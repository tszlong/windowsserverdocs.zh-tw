---
title: 安全地虛擬化 Active Directory 網域服務 (AD DS)
description: USN 復原和 Active Directory 的安全虛擬化
ms.topic: article
ms.prod: windows-server-threshold
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: aa84e09e8a958193fee82c7b9c03cd1dca910c55
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63684198"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>安全地虛擬化 Active Directory 網域服務 (AD DS)

>適用於：Windows Server

AD DS 從 Windows Server 2012 開始，提供更好的支援虛擬化網域控制站，藉由引進安全虛擬化功能。 這篇文章說明角色需要處理 Usn 和 InvocationIDs 在網域控制站複寫，並討論一些潛在的問題，可能會發生。

## <a name="update-sequence-number-and-invocationid"></a>更新序列號碼和 InvocationID

虛擬環境要面對一項挑戰，那就是按照邏輯時鐘複寫配置來分散工作負載。 舉例來說，AD DS 複寫使用指派給每個網域控制站交易的單純遞增值 (稱為 USN 或更新序列號碼)。 每個網域控制站的資料庫執行個體也會提供稱為 InvocationID 的身分。 網域控制站的 InvocationID 與 USN 一起被當做唯一的識別碼，與每個網域控制站上執行的各個寫入交易關聯，而且這個識別碼在樹系內必須也是唯一的。

AD DS 複寫使用每個網域控制站的 InvocationID 與 USN 來判斷哪些變更需要被複寫到其他網域控制站。 如果網域控制站會回復網域控制站感知外及時且 USN 重新用來完全不同的交易，複寫將不會交集，因為其他網域控制站會認為它們已經收到的更新在該 InvocationID 內容下的重複使用 USN 相關聯。

例如，下圖說明當 VDC2 (虛擬機器上執行的目的地網域控制站) 偵測到 USN 回復時，Windows Server 2008 R2 與較舊版作業系統中會發生的事件序列。 在此圖中，偵測到 USN 回復，就會發生在 VDC2 上複寫協力電腦偵測到 VDC2 傳送複寫協力電腦，這表示 VDC2 的資料庫已還原回時間看到最新 USN 值時不正確。

![事件在偵測到 USN 回復時的順序](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

虛擬機器 (VM) 可輕鬆讓 hypervisor 系統管理員，以復原網域控制站的 Usn （它的邏輯時鐘），例如，網域控制站感知外套用快照。 如需 USN 和 USN 回復的詳細資訊，包括示範未偵測到 USN 回復的另一個圖解，請參閱 [USN 和 USN 回復](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

從 Windows Server 2012 開始，裝載於公開 Vm-generation ID 識別碼的 hypervisor 平台的 AD DS 虛擬網域控制站可以偵測並採用必要的安全措施，以便回復虛擬機器時保護 AD DS 環境在 VM 快照集的應用程式的時間。 VM-GenerationID 設計使用 Hypervisor 廠商獨立機制，在來賓虛擬機器的位址空間上顯示這個識別碼，如此一來，只要是支援 VM-GenerationID 的 Hypervisor，都能擁有一致的安全虛擬化經驗。 虛擬機器內部執行的服務及應用程式可以取樣這個識別碼，來偵測虛擬機器是否及時回復。

## <a name="effects-of-usn-rollback"></a>USN 復原的影響

發生 USN 復原時，物件和屬性的修改不是輸入複寫有先前看見該 USN 的目的地網域控制站。

因為這些目的地網域控制站認為他們是最新狀態，在目錄服務事件記錄檔，或所監視和診斷工具，會不報告任何複寫錯誤。

USN 復原可能會影響任何物件或屬性中的任何磁碟分割的複寫。 最常觀察到的副作用是，使用者帳戶及復原網域控制站建立的電腦帳戶不存在於一或多個複寫協力電腦。 或者，您也可以在來自復原網域控制站的密碼更新不存在於複寫協力電腦。

USN 復原可以防止任何使用中的目錄磁碟分割中任何物件類型的複寫。 這些物件類型包括下列各項：

* Active Directory 複寫拓撲及排程
* 這些網域控制站會保留角色與樹系中網域控制站存在
* 樹系中的網域和應用程式磁碟分割存在
* 安全性群組和其目前的群組成員資格的存在
* DNS 記錄登錄在 Active Directory 整合 DNS 區域

USN 漏洞的大小可能代表數百個、 幾千個或甚至是數以萬計的變更，以使用者、 電腦、 信任、 密碼和安全性群組。 USN 漏洞會定義最高 USN 數目時，還原的系統狀態備份，但會變更的原始數字，存在於之前已離線復原網域控制站上建立的差異。

## <a name="detecting-a-usn-rollback"></a>偵測 USN 復原

由於很難偵測到 USN 回復，網域控制站會記錄事件 2095 時來源網域控制站會將先前認可的 USN 號碼傳送至目的地網域控制站，而不需要變更為對應的引動過程識別碼。

若要避免唯一源自更新至 Active Directory 不正確地還原的網域控制站上建立的 Net Logon 服務已暫停。 Net Logon 服務暫停時，使用者和電腦帳戶無法變更的網域控制站上將沒有輸出複寫這類變更的密碼。 同樣地，Active Directory 系統管理工具會在它們進行更新，Active Directory 中的物件而非狀況良好的網域控制站。

在網域控制站，當下列條件成立，會記錄事件訊息，如下所示：

* 來源網域控制站會將先前認可的 USN 號碼傳送至目的地網域控制站。
* 沒有任何變更對應的引動過程識別碼。

目錄服務事件記錄檔中，可能會擷取這些事件。 不過，它們可能會覆寫之前在觀察到由系統管理員。

如果您懷疑 USN 復原有時發生，但不是會看到對應的事件在事件記錄檔，檢查 DSA 無法寫入登錄項目中。 此項目提供鑑識發生 USN 復原的辨識項。

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> 刪除或以手動方式變更 Dsa 無法寫入登錄項目值置於永久不支援的狀態復原網域控制站。 因此，不支援這類變更。 具體而言，修改值中移除的 USN 回復偵測程式碼所加入的隔離行為。 在復原網域控制站上的 Active Directory 資料分割會與相同的 Active Directory 樹系中的直接和可轉移的複寫協力電腦永遠不一致。

此登錄機碼和解決步驟的更多有關可在支援文章[Active Directory 複寫錯誤 8456 或 8457:「 來源 |目的地伺服器目前拒絕複寫要求 「](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination)。

## <a name="virtualization-based-safeguards"></a>基礎的虛擬化保護措施

網域控制站安裝期間，AD DS 一開始會將 VM GenerationID 識別碼儲存 （通常稱為目錄資訊樹狀結構或 DIT） 其資料庫中的網域控制站的電腦物件上 Msds-generationid 屬性的一部分。 VM GenerationID 由虛擬機器中的 Windows 驅動程式獨立追蹤。

當系統管理員從之前的快照還原虛擬機器時，會比對虛擬機器驅動程式 VM GenerationID 的目前值與 DIT 中的值。

如果兩個值不相同，就會重設 InvocationID 並捨棄 RID 集區，以避免重複使用 USN。 如果兩個值是相同的，則會認可交易是正常的。

而且，每次網域控制站重新開機的時候，AD DS 也會比對虛擬機器 VM GenerationID 的目前值與 DIT 中的值，如果不相同，會重設 InvocationID，捨棄 RID 集區，並使用新值更新 DIT。 它還會在未授權狀態下同步化 SYSVOL 資料夾，以便完成安全還原。 這種方式能夠將保護延伸到將快照套用到關機的 VM。 Windows Server 2012 中引入的這些保護措施可讓 AD DS 系統管理員，才會受益於部署和管理虛擬化環境中的網域控制站的獨特優點。

下圖顯示在支援 Vm-generationid 之 hypervisor 執行 Windows Server 2012 的虛擬的網域控制站上偵測到相同的 USN 回復時，如何套用虛擬化保護措施。

![當偵測到相同的 USN 回復時，套用的保護措施](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在這個情況中，當 Hypervisor 偵測到 VM-GenerationID 值變更時，就會觸發虛擬化保護，包括重設虛擬 DC 的 InvocationID (在前面的例子中是從 A 到 B)，然後更新儲存在 VM 上的 VM-GenerationID 值，以便與 Hypervisor 儲存的新值 (G2) 相符。 這些保護可確保兩個網域控制站的複寫能夠交集。

與 Windows Server 2012，AD DS 會使用裝載於 Vm-generationid 感知 hypervisor 的虛擬網域控制站上的保護，並確保意外套用快照集或其他類似啟用 hypervisor 的機制，無法復原的虛擬機器的狀態不會不會破壞 AD DS 環境 （透過防止 USN 泡泡之類的複寫問題，或延遲物件）。

為備份網域控制站的替代機制，不建議藉由套用的虛擬機器快照還原的網域控制站。 建議您繼續使用 Windows Server Backup 或其他 VSS 寫入者備份解決方案。

> [!CAUTION]
> 如果在生產環境中的網域控制站不小心被還原到快照集，它會建議您在應用程式和裝載於該虛擬機器，如需驗證的狀態之後，這些程式的指引服務諮詢供應商快照集還原。

如需詳細資訊，請參閱[虛擬網域控制站安全還原架構](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="recovering-from-a-usn-rollback"></a>復原從 USN 復原

有兩種方法可從 USN 復原：

* 從網域移除網域控制站
* 還原良好備份系統狀態

### <a name="remove-the-domain-controller-from-the-domain"></a>從網域移除網域控制站

1. Active Directory 移除網域控制站，以強制它是獨立的伺服器。
2. 關閉降級的伺服器。
3. 狀況良好的網域控制站，清理降級的網域控制站的中繼資料。
4. 如果不正確地還原的網域控制站主控作業主要角色，將這些角色為狀況良好的網域控制站。
5. 重新啟動降級的伺服器。
6. 如果您需要，請再次在獨立伺服器上安裝 Active Directory。
7. 如果網域控制站先前為通用類別目錄，設定網域控制站是通用類別目錄。
8. 如果網域控制站先前主控作業主要角色，傳送回到網域控制站的主要角色的作業。

### <a name="restore-the-system-state-of-a-good-backup"></a>還原良好備份系統狀態

評估是否有效的系統狀態備份存在此網域控制站。 如果之前已不正確地還原復原網域控制站，且該備份包含最新的網域控制站所做的變更進行有效的系統狀態備份，請從最新的備份還原系統狀態。

您也可以使用快照集作為備份的來源。 您可以設定要成為其本身提供新的引動過程識別碼，使用一節中的程序的資料庫或[時無法使用適當的系統狀態資料的備份，還原虛擬網域控制站](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>後續步驟

* 如需虛擬網域控制站疑難排解的詳細資訊，請參閱[虛擬網域控制站疑難排解](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。
* [Windows Time 服務 (W32Time) 的詳細的資訊](../../networking/windows-time-service/windows-time-service-top.md)
