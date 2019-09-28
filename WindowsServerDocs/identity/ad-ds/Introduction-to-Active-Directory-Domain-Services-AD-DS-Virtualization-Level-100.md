---
title: 安全地虛擬化 Active Directory Domain Services （AD DS）
description: Active Directory 的 USN 復原和安全虛擬化
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 67e35a47467b1f5f66bfd073c6f9db06094ea3f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391034"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>安全地虛擬化 Active Directory Domain Services （AD DS）

>適用於：Windows Server

從 Windows Server 2012 開始，AD DS 藉由引進虛擬化安全功能，提供虛擬化網域控制站的更高支援。 本文說明 Usn 和 InvocationIDs 在網域控制站複寫中的角色，並討論可能發生的一些潛在問題。

## <a name="update-sequence-number-and-invocationid"></a>更新序號和 InvocationID

虛擬環境要面對一項挑戰，那就是按照邏輯時鐘複寫配置來分散工作負載。 舉例來說，AD DS 複寫使用指派給每個網域控制站交易的單純遞增值 (稱為 USN 或更新序列號碼)。 每個網域控制站的資料庫實例也會提供識別，稱為 InvocationID。 網域控制站的 InvocationID 與 USN 一起被當做唯一的識別碼，與每個網域控制站上執行的各個寫入交易關聯，而且這個識別碼在樹系內必須也是唯一的。

AD DS 複寫使用每個網域控制站的 InvocationID 與 USN 來判斷哪些變更需要被複寫到其他網域控制站。 如果網域控制站在網域控制站的感知範圍外回復，而 USN 已重複用於完全不同的交易，則複寫將不會交集，因為其他網域控制站會相信它們已經收到更新在該 InvocationID 的內容下，與重新使用的 USN 相關聯。

例如，下圖說明當 VDC2 (虛擬機器上執行的目的地網域控制站) 偵測到 USN 回復時，Windows Server 2008 R2 與較舊版作業系統中會發生的事件序列。 在此圖中，當複寫協力電腦偵測到 VDC2 已傳送複寫協力電腦先前所見的最新 USN 值（表示 VDC2's 資料庫已準時回復）時，就會在 VDC2 上進行 USN 復原的偵測。當地.

![偵測到 USN 回復時的事件順序](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

虛擬機器（VM）可讓管理者系統管理員輕鬆復原網域控制站的 Usn （其邏輯時鐘），例如，在網域控制站的感知範圍外套用快照集。 如需 USN 和 USN 回復的詳細資訊，包括示範未偵測到 USN 回復的另一個圖解，請參閱 [USN 和 USN 回復](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

從 Windows Server 2012 開始，AD DS 裝載在虛擬機器世代識別碼中的虛擬網域控制站，可以偵測並採用必要的安全措施，以保護 AD DS 環境（如果已匯總虛擬機器）VM 快照集的應用程式恢復時間。 VM-GenerationID 設計使用 Hypervisor 廠商獨立機制，在來賓虛擬機器的位址空間上顯示這個識別碼，如此一來，只要是支援 VM-GenerationID 的 Hypervisor，都能擁有一致的安全虛擬化經驗。 虛擬機器內部執行的服務及應用程式可以取樣這個識別碼，來偵測虛擬機器是否及時回復。

## <a name="effects-of-usn-rollback"></a>USN 復原的效果

發生 USN 回復時，物件和屬性的修改不會由先前看過 USN 的目的地網域控制站進行輸入複寫。

因為這些目的地網域控制站認為它們是最新狀態，所以在目錄服務事件記錄檔中或透過監視和診斷工具不會報告複寫錯誤。

USN 復原可能會影響任何分割區中任何物件或屬性的複寫。 最常觀察的副作用是，在復原網域控制站上建立的使用者帳戶和電腦帳戶不存在於一或多個複寫協力電腦上。 或者，在復原網域控制站上產生的密碼更新不存在於複寫協力電腦上。

USN 復原可以防止任何 Active Directory 分割區中的任何物件類型進行複寫。 這些物件類型包括下列各項：

* Active Directory 複製拓朴和排程
* 樹系中的網域控制站和這些網域控制站保存的角色是否存在
* 樹系中的網域和應用程式磁碟分割是否存在
* 安全性群組和其目前的群組成員資格是否存在
* Active Directory 整合的 DNS 區域中的 DNS 記錄註冊

USN 洞的大小可能代表數百、數千或甚至數萬的使用者、電腦、信任、密碼和安全性群組變更。 USN 漏洞的定義是在進行還原的系統狀態備份時，所存在的最高 USN 號碼與在其離線之前，已在回復網域控制站上建立的原始變更數目之間的差異。

## <a name="detecting-a-usn-rollback"></a>偵測 USN 復原

由於 USN 復原很容易偵測，因此當來源網域控制站將先前認可的 USN 號碼傳送至目的地網域控制站時，如果調用識別碼中沒有對應的變更，網域控制站就會記錄事件2095。

為避免在不正確還原的網域控制站上建立 Active Directory 的唯一原始更新，Net Logon 服務已暫停。 當 Net Logon 服務暫停時，使用者和電腦帳戶無法變更不會輸出複寫這類變更之網域控制站上的密碼。 同樣地，Active Directory 管理工具在對 Active Directory 中的物件進行更新時，會優先使用狀況良好的網域控制站。

在網域控制站上，如果下列條件成立，則會記錄類似下列的事件訊息：

* 來源網域控制站會將先前認可的 USN 號碼傳送至目的地網域控制站。
* 調用識別碼中沒有對應的變更。

這些事件可能會在目錄服務事件記錄檔中捕捉。 不過，系統管理員可能會覆寫它們，然後才會觀察到它們。

如果您懷疑 USN 復原已經發生，但是在事件記錄檔中看不到對應的事件，請檢查登錄中是否有 [DSA 不可寫入] 專案。 此專案會提供已發生 USN 復原的辨識證明。

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> 刪除或手動變更 [Dsa 不可寫入] 登錄專案值，會使復原網域控制站處於永久不受支援的狀態。 因此，不支援這類變更。 具體而言，修改此值會移除 USN 復原偵測程式碼所新增的隔離行為。 復原網域控制站上的 Active Directory 分割區，會與相同 Active Directory 樹系中的直接和可轉移複寫協力電腦永久不一致。

如需此登錄機碼和解決步驟的詳細資訊，請參閱支援文章 [Active 目錄複寫錯誤8456或8457：「來源 |目的地伺服器目前拒絕複寫要求 "](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination)。

## <a name="virtualization-based-safeguards"></a>以虛擬化為基礎的保護

在網域控制站安裝期間，AD DS 一開始會在網域控制站的電腦物件（通常稱為目錄資訊樹狀目錄或 DIT）中，將 VM GenerationID 識別碼儲存為 GenerationID 屬性的一部分。 VM GenerationID 由虛擬機器中的 Windows 驅動程式獨立追蹤。

當系統管理員從之前的快照還原虛擬機器時，會比對虛擬機器驅動程式 VM GenerationID 的目前值與 DIT 中的值。

如果兩個值不相同，就會重設 InvocationID 並捨棄 RID 集區，以避免重複使用 USN。 如果兩個值是相同的，則會認可交易是正常的。

而且，每次網域控制站重新開機的時候，AD DS 也會比對虛擬機器 VM GenerationID 的目前值與 DIT 中的值，如果不相同，會重設 InvocationID，捨棄 RID 集區，並使用新值更新 DIT。 它還會在未授權狀態下同步化 SYSVOL 資料夾，以便完成安全還原。 這種方式能夠將保護延伸到將快照套用到關機的 VM。 Windows Server 2012 中引進的這些保護措施，讓 AD DS 系統管理員可以受益于在虛擬化環境中部署和管理網域控制站的獨特優點。

下圖顯示在支援 GenerationID 的虛擬機器上執行 Windows Server 2012 的虛擬網域控制站上，偵測到相同的 USN 回復時，如何套用虛擬化保護措施。

![偵測到相同的 USN 回復時套用的保護措施](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在這個情況中，當 Hypervisor 偵測到 VM-GenerationID 值變更時，就會觸發虛擬化保護，包括重設虛擬 DC 的 InvocationID (在前面的例子中是從 A 到 B)，然後更新儲存在 VM 上的 VM-GenerationID 值，以便與 Hypervisor 儲存的新值 (G2) 相符。 這些保護可確保兩個網域控制站的複寫能夠交集。

在 Windows Server 2012 中，AD DS 會在裝載于 VM GenerationID 感知管理元件上的虛擬網域控制站上採用保護措施，並確保意外地應用快照集或其他可能會復原虛擬機器的啟用程式集型機制機器的狀態不會中斷 AD DS 環境（藉由防止 USN 反升或延遲物件之類的複寫問題）。

不建議透過套用虛擬機器快照集來還原網域控制站，做為備份網域控制站的替代機制。 建議您繼續使用 Windows Server Backup 或其他 VSS 寫入者備份解決方案。

> [!CAUTION]
> 如果生產環境中的網域控制站不小心還原為快照集，建議您查閱應用程式的廠商，以及該虛擬機器上裝載的服務，以取得在之後驗證這些程式狀態的指引快照集還原。

如需詳細資訊，請參閱[虛擬網域控制站安全還原架構](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="recovering-from-a-usn-rollback"></a>從 USN 回復復原

有兩種方法可以從 USN 復原復原：

* 從網域移除網域控制站
* 還原良好備份的系統狀態

### <a name="remove-the-domain-controller-from-the-domain"></a>從網域移除網域控制站

1. 從網域控制站移除 Active Directory，強制它成為獨立伺服器。
2. 關閉降級的伺服器。
3. 在狀況良好的網域控制站上，清除降級網域控制站的中繼資料。
4. 如果未正確還原的網域控制站裝載操作主機角色，請將這些角色轉移至狀況良好的網域控制站。
5. 重新開機降級的伺服器。
6. 如有需要，請在獨立伺服器上再次安裝 Active Directory。
7. 如果網域控制站先前是通用類別目錄，請將網域控制站設定為通用類別目錄。
8. 如果網域控制站先前裝載操作主機角色，請將操作主機角色轉移回網域控制站。

### <a name="restore-the-system-state-of-a-good-backup"></a>還原良好備份的系統狀態

評估此網域控制站是否有有效的系統狀態備份。 如果在復原的網域控制站未正確還原之前進行了有效的系統狀態備份，而且備份包含在網域控制站上所做的最近變更，請從最新的備份還原系統狀態。

您也可以使用快照集做為備份的來源。 或者，您可以使用在[適當的系統狀態資料備份無法使用時還原虛擬網域控制站](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)一節中的程式，將資料庫設定為本身提供新的調用識別碼

## <a name="next-steps"></a>後續步驟

* 如需虛擬網域控制站疑難排解的詳細資訊，請參閱[虛擬網域控制站疑難排解](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。
* [Windows 時間服務（W32Time）的詳細資訊](../../networking/windows-time-service/windows-time-service-top.md)
