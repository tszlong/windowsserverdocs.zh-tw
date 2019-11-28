---
title: 安全的虛擬化 Active Directory Domain Services (AD DS)
description: USN 回復和安全的 Active Directory 虛擬化
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
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391034"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>安全的虛擬化 Active Directory Domain Services (AD DS)

>適用於：Windows Server

從 Windows Server 2012 開始，為了對網域控制站的虛擬化提供更好的支援，AD DS 引入了可以安全虛擬化的功能。 本文說明 USN 和 InvocationID 在網域控制站複寫中的角色，並討論可能發生的一些潛在問題。

## <a name="update-sequence-number-and-invocationid"></a>更新序號和 InvocationID

虛擬環境要面對一項挑戰，那就是按照邏輯時鐘複寫配置來分散工作負載。 舉例來說，AD DS 複寫使用指派給每個網域控制站交易的單純遞增值 (稱為 USN 或更新序列號碼)。 系統會指定一個身分識別 (稱為 InvocationID) 給每個網域控制站的資料庫執行個體。 網域控制站的 InvocationID 與 USN 一起被當做唯一的識別碼，與每個網域控制站上執行的各個寫入交易關聯，而且這個識別碼在樹系內必須也是唯一的。

AD DS 複寫使用每個網域控制站的 InvocationID 與 USN 來判斷哪些變更需要被複寫到其他網域控制站。 如果網域控制站在網域控制站感知外及時回復，並且將 USN 重新用在一個完全不同的交易，那麼複寫將不會交集，因為在該 InvocationID 內容下，其他網域控制站會認為它們已經收到與這個重複使用 USN 相關聯的更新。

例如，下圖說明當 VDC2 (虛擬機器上執行的目的地網域控制站) 偵測到 USN 回復時，Windows Server 2008 R2 與較舊版作業系統中會發生的事件序列。 在此圖中，當複寫協力電腦偵測到 VDC2 傳送了一個複寫協力電腦之前已經看到的最新 USN 值時，就會在 VDC2 上進行 USN 回復偵測，這表示 VDC2 的資料庫沒有適當地及時回復。

![偵測到 USN 回復時的事件順序](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

虛擬機器 (VM) 可讓 Hypervisor 系統管理員輕鬆回復網域控制站的 USN (它的邏輯時鐘)，例如，在網域控制站感知外套用快照。 如需 USN 和 USN 回復的詳細資訊，包括示範未偵測到 USN 回復的另一個圖解，請參閱 [USN 和 USN 回復](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

從 Windows Server 2012  開始，裝載於 Hypervisor 平台 (會公開 VM-Generation ID 識別碼) 上的 AD DS 虛擬網域控制站，可以偵測並採用必要的安全措施，以便在套用 VM 快照後及回復虛擬機器時保護 AD DS 環境。 VM-GenerationID 設計使用 Hypervisor 廠商獨立機制，在來賓虛擬機器的位址空間上顯示這個識別碼，如此一來，只要是支援 VM-GenerationID 的 Hypervisor，都能擁有一致的安全虛擬化經驗。 虛擬機器內部執行的服務及應用程式可以取樣這個識別碼，來偵測虛擬機器是否及時回復。

## <a name="effects-of-usn-rollback"></a>USN 回復的效果

發生 USN 回復時，物件和屬性的修改不會由先前看過 USN 的目的地網域控制站進行輸入複寫。

因為這些目的地網域控制站認為他們是最新的，所以在目錄服務事件記錄中或從監視及診斷工具，不會回報任何複寫錯誤。

USN 回復可能會影響任何分割區中任何物件或屬性的複寫。 最常見的副作用是，在回復網域控制站上建立的使用者帳戶和電腦帳戶，在一或多個複寫協力電腦上不存在。 或者，在回復網域控制站上產生的密碼更新，不存在於複寫協力電腦上。

USN 回復可以防止任何 Active Directory 分割區中的任何物件類型進行複寫。 這些物件類型包括：

* Active Directory 複寫拓樸和排程
* 樹系中的網域控制站以及這些網域控制站保存的角色的存在
* 樹系中網域和應用程式分割區的存在
* 安全性群組及其目前群組成員資格的存在
* Active Directory 整合 DNS 區域中的 DNS 記錄註冊

USN 洞的大小可能代表數百、數千、甚至數萬的使用者、電腦、信任、密碼和安全性群組的變更。 USN 洞的定義是：在進行還原的系統狀態備份時所存在的最高 USN 號碼，以及在回復的網域控制站離線之前已建立的原始變更數目，這兩個數字之間的差。

## <a name="detecting-a-usn-rollback"></a>偵測 USN 回復

由於 USN 回復很難偵測到，因此當來源網域控制站將先前認可的 USN 號碼傳送至目的地網域控制站時，如果呼叫識別碼中沒有對應的變更，網域控制站就會記錄 2095 事件。

為了避免在不正確的還原網域控制站上建立 Active Directory 的唯一原始更新，會暫停 Net Logon 服務。 當 Net Logon 服務暫停時，使用者和電腦帳戶無法變更不會輸出複寫這類變更之網域控制站的密碼。 同樣地，Active Directory 管理工具在對 Active Directory 中的物件進行更新時，會優先使用狀況良好的網域控制站。

在網域控制站上，如果下列條件成立，則會記錄類似下列的事件訊息：

* 來源網域控制站會將先前認可的 USN 號碼傳送至目的地網域控制站。
* 呼叫識別碼中沒有對應的變更。

這些事件可能可在目錄服務的事件記錄檔中擷取。 不過，系統管理員可能還沒觀察到就先覆寫掉它們了。

如果您懷疑 USN 回復已經發生，但是在事件記錄檔中沒看到對應的事件，請檢查登錄中是否有 [DSA Not Writable] (DSA 不可寫入) 項目。 此項目可提供 USN 回復已發生的鐵證。

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> 刪除或手動變更 [DSA Not Writable] 登錄項目值，會使回復網域控制站處於永久不受支援的狀態。 因此，不支援這樣的變更。 具體而言，修改此值會移除 USN 回復偵測程式碼所增加的隔離行為。 回復網域控制站上的 Active Directory 分割區，與同一 Active Directory 樹系中的直接和轉移複寫協力電腦將永遠不一致。

如需此登錄機碼和解決步驟的詳細資訊，請參閱支援文章 [Active Directory 複寫錯誤 8456 或 8457：「來源 |目的地伺服器目前拒絕複寫要求」](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination)。

## <a name="virtualization-based-safeguards"></a>虛擬化型安全性

安裝網域控制站期間，AD DS 一開始就會將 VM GenerationID 識別碼儲存到資料庫 (通常就是指目錄資訊樹狀結構或 DIT)，做為網域控制站電腦物件上 msDS-GenerationID 屬性的一部分。 VM GenerationID 由虛擬機器中的 Windows 驅動程式獨立追蹤。

當系統管理員從之前的快照還原虛擬機器時，會比對虛擬機器驅動程式 VM GenerationID 的目前值與 DIT 中的值。

如果兩個值不相同，就會重設 InvocationID 並捨棄 RID 集區，以避免重複使用 USN。 如果兩個值是相同的，則會認可交易是正常的。

而且，每次網域控制站重新開機的時候，AD DS 也會比對虛擬機器 VM GenerationID 的目前值與 DIT 中的值，如果不相同，會重設 InvocationID，捨棄 RID 集區，並使用新值更新 DIT。 它還會在未授權狀態下同步化 SYSVOL 資料夾，以便完成安全還原。 這種方式能夠將保護延伸到將快照套用到關機的 VM。 Windows Server 2012 引進的這些保護，可讓 AD DS 系統管理員受益於在虛擬環境中部署及管理網域控制站的獨特好處。

下圖顯示在支援 VM-GenerationID 之 Hypervisor 上執行 Windows Server 2012 的虛擬網域控制站偵測到相同的 USN 回復時，是如何套用虛擬化保護的。

![偵測到相同的 USN 回復時套用的保護措施](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在這個情況中，當 Hypervisor 偵測到 VM-GenerationID 值變更時，就會觸發虛擬化保護，包括重設虛擬 DC 的 InvocationID (在前面的例子中是從 A 到 B)，然後更新儲存在 VM 上的 VM-GenerationID 值，以便與 Hypervisor 儲存的新值 (G2) 相符。 這些保護可確保兩個網域控制站的複寫能夠交集。

在 Windows Server 2012 中，AD DS 會在裝載於 VM-GenerationID 感知 Hypervisor 的虛擬網域控制站上使用保護，並確保意外套用快照或可能「回復」虛擬機器狀態的其他類似啟用 Hypervisor 的機制，不會破壞 AD DS 環境 (透過防止 USN 泡泡或延遲物件之類的複寫問題)。

不建議將套用虛擬機器快照來還原網域控制站的方式當做備份網域控制站的替代機制。 建議您繼續使用 Windows Server Backup 或其他 VSS 寫入者備份解決方案。

> [!CAUTION]
> 如果實際執行環境中的網域控制站不小心被還原成快照，建議您諮詢廠商該虛擬機器上裝載的應用程式和服務相關事宜，取得快照還原之後確認這些程式狀態的指示說明。

如需詳細資訊，請參閱[虛擬網域控制站安全還原架構](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="recovering-from-a-usn-rollback"></a>從 USN 回復復原

有兩種方法可以從 USN 回復復原：

* 從網域中移除網域控制站
* 從好的備份還原系統狀態

### <a name="remove-the-domain-controller-from-the-domain"></a>從網域中移除網域控制站

1. 從網域控制站中移除 Active Directory，強制它成為獨立伺服器。
2. 關閉被降級的伺服器。
3. 在狀況良好的網域控制站上，清除降級網域控制站的中繼資料。
4. 如果未正確還原的網域控制站裝載了操作主機角色，請將這些角色轉移至狀況良好的網域控制站。
5. 重新啟動降級的伺服器。
6. 如有需要，可在獨立伺服器上再次安裝 Active Directory。
7. 如果網域控制站之前是通用目錄，請將網域控制站設定為通用目錄。
8. 如果網域控制站之前有裝載操作主機角色，請將操作主機角色轉移回網域控制站。

### <a name="restore-the-system-state-of-a-good-backup"></a>從好的備份還原系統狀態

評估此網域控制站是否有有效的系統狀態備份。 如果在回復的網域控制站未正確還原之前，有進行有效的系統狀態備份，而且備份包含網域控制站最近所做的變更，請從最新的備份還原系統狀態。

您也可以使用快照做為備份的來源。 或者，利用[在沒有適當系統狀態資料備份的情況下還原虛擬網域控制站](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)中的程序，將資料庫設定成為其本身提供新的引動過程識別碼。

## <a name="next-steps"></a>後續步驟

* 如需虛擬網域控制站疑難排解的詳細資訊，請參閱[虛擬網域控制站疑難排解](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。
* [Windows Time Service (W32Time) 的詳細資訊](../../networking/windows-time-service/windows-time-service-top.md)
