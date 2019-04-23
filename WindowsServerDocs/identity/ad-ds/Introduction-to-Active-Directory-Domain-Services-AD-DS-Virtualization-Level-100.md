---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: Active Directory Domain Services (AD DS) 虛擬化 (等級 100) 簡介
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b818ba5a58db38bdb3c0f630a8d9d2daa1494403
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878089"
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Active Directory Domain Services (AD DS) 虛擬化 (等級 100) 簡介

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Active Directory 網域服務 (AD DS) 環境的虛擬化已經持續好幾年了。 AD DS 從 Windows Server 2012 開始，提供更好的支援虛擬化網域控制站，藉由引進安全虛擬化功能。

## <a name="safe-virtualization-of-domain-controllers"></a>網域控制站的安全虛擬化

虛擬環境要面對一項挑戰，那就是按照邏輯時鐘複寫配置來分散工作負載。 舉例來說，AD DS 複寫使用指派給每個網域控制站交易的單純遞增值 (稱為 USN 或更新序列號碼)。 每個網域控制站的資料庫執行個體也會提供稱為 InvocationID 的身分。 網域控制站的 InvocationID 與 USN 一起被當做唯一的識別碼，與每個網域控制站上執行的各個寫入交易關聯，而且這個識別碼在樹系內必須也是唯一的。

AD DS 複寫使用每個網域控制站的 InvocationID 與 USN 來判斷哪些變更需要被複寫到其他網域控制站。 如果網域控制站會回復網域控制站感知外及時且 USN 重新用來完全不同的交易，複寫將不會交集，因為其他網域控制站會認為它們已經收到的更新在該 InvocationID 內容下的重複使用 USN 相關聯。

例如，下圖說明當 VDC2 (虛擬機器上執行的目的地網域控制站) 偵測到 USN 回復時，Windows Server 2008 R2 與較舊版作業系統中會發生的事件序列。 在此圖中，偵測到 USN 回復，就會發生在 VDC2 上複寫協力電腦偵測到 VDC2 傳送複寫協力電腦，這表示 VDC2 的資料庫已還原回時間看到最新 USN 值時不正確。

![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

虛擬機器 (VM) 可輕鬆讓 hypervisor 系統管理員，以復原網域控制站的 Usn （它的邏輯時鐘），例如，網域控制站感知外套用快照。 如需 USN 和 USN 回復的詳細資訊，包括示範未偵測到 USN 回復的另一個圖解，請參閱 [USN 和 USN 回復](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

從 Windows Server 2012 開始，裝載於公開 Vm-generation ID 識別碼的 hypervisor 平台的 AD DS 虛擬網域控制站可以偵測並採用必要的安全措施，以便回復虛擬機器時保護 AD DS 環境在 VM 快照集的應用程式的時間。 VM-GenerationID 設計使用 Hypervisor 廠商獨立機制，在來賓虛擬機器的位址空間上顯示這個識別碼，如此一來，只要是支援 VM-GenerationID 的 Hypervisor，都能擁有一致的安全虛擬化經驗。 虛擬機器內部執行的服務及應用程式可以取樣這個識別碼，來偵測虛擬機器是否及時回復。

### <a name="BKMK_HowSafeguardsWork"></a>這些虛擬化保護措施如何運作？
網域控制站安裝期間，AD DS 一開始會將 VM GenerationID 識別碼儲存 （通常稱為目錄資訊樹狀結構或 DIT） 其資料庫中的網域控制站的電腦物件上 Msds-generationid 屬性的一部分。 VM GenerationID 由虛擬機器中的 Windows 驅動程式獨立追蹤。

當系統管理員從之前的快照還原虛擬機器時，會比對虛擬機器驅動程式 VM GenerationID 的目前值與 DIT 中的值。

如果兩個值不相同，就會重設 InvocationID 並捨棄 RID 集區，以避免重複使用 USN。 如果兩個值是相同的，則會認可交易是正常的。

而且，每次網域控制站重新開機的時候，AD DS 也會比對虛擬機器 VM GenerationID 的目前值與 DIT 中的值，如果不相同，會重設 InvocationID，捨棄 RID 集區，並使用新值更新 DIT。 它還會在未授權狀態下同步化 SYSVOL 資料夾，以便完成安全還原。 這種方式能夠將保護延伸到將快照套用到關機的 VM。 Windows Server 2012 中引入的這些保護措施可讓 AD DS 系統管理員，才會受益於部署和管理虛擬化環境中的網域控制站的獨特優點。

下圖顯示在支援 Vm-generationid 之 hypervisor 執行 Windows Server 2012 的虛擬的網域控制站上偵測到相同的 USN 回復時，如何套用虛擬化保護措施。

![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

在這個情況中，當 Hypervisor 偵測到 VM-GenerationID 值變更時，就會觸發虛擬化保護，包括重設虛擬 DC 的 InvocationID (在前面的例子中是從 A 到 B)，然後更新儲存在 VM 上的 VM-GenerationID 值，以便與 Hypervisor 儲存的新值 (G2) 相符。 這些保護可確保兩個網域控制站的複寫能夠交集。

與 Windows Server 2012，AD DS 會使用裝載於 Vm-generationid 感知 hypervisor 的虛擬網域控制站上的保護，並確保意外套用快照集或其他類似啟用 hypervisor 的機制，無法復原的虛擬機器的狀態不會不會破壞 AD DS 環境 （透過防止 USN 泡泡之類的複寫問題，或延遲物件）。 不過，不建議將套用虛擬機器快照來還原網域控制站的方式當做備份網域控制站的替代機制。 建議您繼續使用 Windows Server Backup 或其他 VSS 寫入者備份解決方案。

> [!CAUTION]
> 如果在生產環境中的網域控制站不小心被還原到快照集，它會建議您在應用程式和裝載於該虛擬機器，如需驗證的狀態之後，這些程式的指引服務諮詢供應商快照集還原。

如需詳細資訊，請參閱[虛擬網域控制站安全還原架構](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="virtualized_dc_cloning"></a>複製虛擬的網域控制站
從 Windows Server 2012 開始，系統管理員可以輕鬆且安全地部署複本網域控制站複製現有的虛擬網域控制站。 在虛擬環境中，系統管理員不用再使用 sysprep.exe 重複地部署所準備的伺服器映像，將伺服器升級為網域控制站，然後完成部署每個複本網域控制站時所需的額外設定需求。

> [!NOTE]
> 系統管理員需要按照現有的程序在網域中部署第一個網域控制站，像是使用 sysprep.exe 來準備伺服器虛擬硬碟 (VHD)，將伺服器升級為網域控制站，然後完成任何額外的設定要求。 在嚴重損壞修復的狀況中，使用最新的伺服器備份來還原網域中的第一個網域控制站。

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>受益於虛擬網域控制站複製的案例

-   在新網域中快速部署其他網域控制站

-   在嚴重損壞修復期間，使用複製來快速部署網域控制站以還原 AD DS 容量，迅速還原公司業務的持續性。

-   利用網域控制站的彈性佈建來最佳化私人雲端部署，以便容納規模擴大的需求

-   快速佈建測試環境能夠在實際執行展開之前，先行部署及測試新的功能

-   在分公司複製現有的網域控制站，能夠快速滿足分公司日漸增加的容量需求

快速部署大量網域控制站的時候，繼續按照安裝完成之後驗證每個網域控制站健康情況的現有程序執行。 以合理的批次大小來部署網域控制站，如此即可在每個批次安裝完成之後驗證它們的健康情況。 建議使用的批次大小為 10。 如需詳細資訊，請參閱[部署複製虛擬網域控制站的步驟](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)。

### <a name="clear-separation-of-responsibilities"></a>清楚劃分責任
複製虛擬網域控制站的授權是在 AD DS 系統管理員的控制之下。 為了讓 Hypervisor 系統管理員能夠透過複製虛擬網域控制站的方式來部署其他網域控制站，AD DS 系統管理員必須選取並授權一個網域控制站，然後執行準備步驟以便讓它成為複製來源。

因為虛擬機器佈建通常都由 Hypervisor 系統管理員負責管理，所以他們可以複製 AD DS 系統管理員授權且準備好用來複製的虛擬網域控制站，來佈建複本網域控制站虛擬機器。

> [!WARNING]
> 允許管理裝載虛擬網域控制站之 Hypervisor 的任何人，在環境中都必須具有極高的可信度且能夠通過稽核。

### <a name="how-does-virtual-domain-controller-cloning-work"></a>虛擬網域控制站複製如何運作？
複製程序牽涉到建立複本的現有虛擬網域控制站的 VHD （若是更複雜的設定，網域控制站 VM），授權它在 AD DS 中複製和建立複製設定檔。 這個程序可以免除重複執行的部署工作，減少複本虛擬網域控制站的部署步驟及時間。

複製網域控制站使用下列條件，偵測它是否是另一個網域控制站的複本：

1.  虛擬機器提供的 VM-Generation ID 值與 DIT 中儲存的 VM-Generation ID 值不相同。

    > [!NOTE]
    > Hypervisor 平台必須支援 Vm-generation ID （Windows Server 2012 HYPER-V 支援 Vm-generation ID）。

2.  下列其中一個位置中有名為 DCCloneConfig.xml 的檔案：

    -   DIT 所在的目錄

    -   %windir%\NTDS

    -   卸除式媒體磁碟機的根目錄

滿足這些條件之後，它就會進行複製程序，將它自己佈建為複本網域控制站。

複製網域控制站使用來源網域控制站 （它代表其複本） 的安全性內容 （也稱為連絡 Windows Server 2012 網域控制站 (PDC) 模擬器操作主機角色持有者彈性單一主機操作或 FSMO）。 PDC 模擬器必須執行 Windows Server 2012，但它並沒有在 hypervisor 上執行。

> [!NOTE]
> 如果您有結構描述延伸，其中含有參考來源網域控制站的屬性，且屬性位於被複製來建立複製的其中一個物件 (電腦物件、NTDS 設定物件) 上，則不會複製或更新該屬性來參考複製網域控制站。

確認要求網域控制站具備複製所需的授權後，PDC 模擬器會建立一個新的機器身分識別，其中包含新的帳戶、SID、名稱及密碼，用來識別該機器為複本網域控制站，並將這些資訊傳送回複製網域控制站。 接著複製網域控制站會準備 AD DS 資料庫檔案做為複本，也會清除機器狀態。

如需詳細資訊，請參閱 [Virtualized domain controller cloning architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)。

### <a name="cloning-components"></a>複製元件
複製元件包括 Windows PowerShell 的 Active Directory 模組中新的 Cmdlet 及關聯的 XML 檔案：

-   **New-addccloneconfigfile** 」 這個指令程式會建立，並將 DCCloneConfig.xml 放在正確的位置，以確保它可以用來觸發複製。 它也會執行先決條件檢查以確保成功複製。 它內含於 Windows PowerShell 的 Active Directory 模組中。 您可以在準備用來複製的虛擬網域控制站本機執行它，也可以使用 -offline 選項從遠端執行它。 您可以指定複製網域控制站的設定，如控制站的名稱、站台及 IP 位址。

    它執行的先決條件檢查包括：

    > [!NOTE]
    > 先決條件檢查未執行時 「 使用 offline 選項。 如需詳細資訊，請參閱[在離線模式中執行 New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode)。

    -   準備好的 DC 具備複製的權限 (是 [Cloneable Domain Controllers] 群組的成員)

    -   PDC 模擬器執行 Windows Server 2012。

    -   執行 **Get-ADDCCloningExcludedApplicationList** 所列示的任何程式或服務都會包含於 CustomDCCloneAllowList.xml (在這份複製元件清單最後會有更為詳細的說明)。

-   **DCCloneConfig.xml** 「 為了成功複製虛擬的網域控制站，這個檔案必須存在於 DIT 所在的目錄 *%windir%\NTDS*，或卸除式媒體磁碟機的根目錄。 該檔案除了被做為偵測及啟動複製的其中一個觸發程序以外，它也提供了指定複製網域控制站組態設定的方法。

    結構描述及範例檔 DCCloneConfig.xml 檔案會儲存在所有 Windows Server 2012 電腦上：

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    建議您使用 New-ADDCCloneConfigFile Cmdlet 來建立 DCCloneConfig.xml 檔案。 雖然您也可以使用 XML 感知編輯器處理結構描述檔來建立這個檔案，但是手動編輯檔案很容易出現錯誤。 如果您要編輯該檔案，必須使用 XML 感知編輯器，例如 Visual Studio、 [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)，或第三方應用程式 (請勿使用 [記事本])。

-   **Get-addccloningexcludedapplicationlist** 」 執行此 cmdlet 在來源網域控制站上開始複製程序，以判斷哪些服務或已安裝的程式不支援的預設值 清單中，在之前DefaultDCCloneAllowList.xml 或使用者定義的包含清單名稱為的 CustomDCCloneAllowList.xml 檔案，並因而尚未被估評複製影響。

    這個 Cmdlet 會在來源網域控制站上搜尋服務控制管理員中的服務，並在 **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** 之下，搜尋預設清單 (DefaultDCCloneAllowList.xml) 或使用者定義包含清單 (CustomDCCloneAllowList.xml 檔案) (如果有提供的話) 中沒有指定的已安裝程式。 執行該 Cmdlet 後傳回的應用程式及服務清單，是 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 檔案中已經提供的項目，與執行階段期間根據來源 DC 安裝項目所建構之清單間的差異。 從 Get-ADDCCloningExcludedApplicationList 輸出的服務和程式，如果您判斷它們可以被順利複製，則可以加入到 CustomDCCloneAllowList.xml 檔案。 若要判斷服務或已安裝程式是否可以被順利複製，請評估下列情況：

    -   服務或已安裝程式是否會受機器身分識別 (如名稱、SID、密碼等) 影響？

    -   服務或已安裝程式是否在本機電腦上儲存了複製時會影響其功能的任何狀態？

    您必須與應用程式的軟體廠商配合，判斷服務或程式是否可以被順利複製。

    > [!NOTE]
    > 在 CustomDCCloneAllowList.xml 檔案中佈建其他服務或程式之前，請確認您是否擁有所需的授權來複製該虛擬機器上的這個軟體。

    如果應用程式是不可複製的，請先從來源網域控制站移除它們，然後才建立複製媒體。 如果某個應用程式出現在 Cmdlet 輸出，但卻沒有包含在 CustomDCCloneAllowList.xml 檔案中，則複製會失敗。 為了成功進行複製，Cmdlet 輸出中不應該列示任何服務或程式。 換句話說，應用程式應該被包含在 CustomDCCloneAllowList.xml 檔案中，不然就應該從來源網域控制站移除。

    下表說明執行 Get-ADDCCloningExcludedApplicationList 的選項。

    |||
    |-|-|
    |引數|說明|
    |*<no argument specified>*|顯示主控台上尚未指明可以被複製之服務或程式的清單。 如果任何可能的位置中已經有 CustomDCCloneAllowList.XML，它會使用該檔案來顯示其餘的服務和程式 (如果清單相符的話，不會顯示任何項目)。|
    |-GenerateXml|建立填入了主控台上列示之服務和程式的 CustomDCCloneAllowList.XML 檔案。|
    |-Force|覆寫現有的 CustomDCCloneAllowList.XML 檔案。|
    |-Path|建立 CustomDCCloneAllowList.XML 的資料夾路徑。|

-   **DefaultDCCloneAllowList.xml** 「 這個檔案存在於預設每個 Windows Server 2012 網域控制站 in *%windir%\system32*。 它預設會列示可以順利複製的服務和已安裝程式。 您不可以變更此檔案的位置或內容，否則複製會失敗。

-   **CustomDCCloneAllowList.xml** 」 如果您有服務或安裝的程式，您的來源網域控制站上以外 DefaultDCCloneAllowList.xml 檔案中列出的這些服務或程式必須包含在此檔案。 若要尋找 DefaultDCCloneAllowList.xml 檔案中沒有列示的服務或已安裝程式，請執行 **Get-ADDCCloningExcludedApplicationList** Cmdlet。 您應該使用 **"GenerateXml**引數來產生 XML 檔案。

    複製程序會檢查下列位置，讓這個檔案使用所找到的第一個 XML 檔案，無論其他資料夾的內容是什麼：

    1.  下列登錄機碼：

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  DSA 工作目錄

    3.  %systemroot%\NTDS

    4.  依磁碟機代號順序的卸除式讀/寫媒體，位於磁碟機的根目錄

### <a name="deployment-scenarios"></a>部署案例
虛擬網域控制站複製支援下列部署案例：

-   部署複製網域控制站建立的來源網域控制站的虛擬硬碟 (vhd) 檔案的複本。

-   使用 Hypervisor 公開的匯出/匯入語意來複製來源網域控制站的虛擬機器，然後部署複製網域控制站。

> [!NOTE]
> 一節中的步驟[部署複製虛擬的網域控制站的步驟](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)示範複製使用匯出/匯入功能的 Windows Server 2012 HYPER-V 虛擬機器。

## <a name="steps_deploy_vdc"></a>部署複製虛擬的網域控制站的步驟

-   [必要條件](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [步驟 1：要複製的權限授與來源虛擬的網域控制站](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [步驟 2：Run Get-ADDCCloningExcludedApplicationList cmdlet](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [步驟 3：執行 New-addccloneconfigfile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [步驟 4：匯出，然後再匯入虛擬機器的來源網域控制站](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>必要條件

-   您必須是 Domain Admins 群組的成員或具備與指派之權限相等的權限，才能完成下列程序中的步驟。

-   必須從提升權限的命令提示字元執行本指南中使用的 Windows PowerShell 命令。 若要這樣做，請以滑鼠右鍵按一下**Windows PowerShell**圖示，，然後按一下**系統管理員身分執行**。

-   安裝 HYPER-V 伺服器角色的 Windows Server 2012 伺服器 (**HyperV1**)。

-   安裝 HYPER-V 伺服器角色的第二個 Windows Server 2012 伺服器 (**HyperV2**)。

    > [!NOTE]
    > -   如果您使用另一個 Hypervisor，您應該連絡該 Hypervisor 的廠商，確認該 Hypervisor 是否支援 VM-Generation ID。 如果這個 Hypervisor 不支援 VM-Generation ID 而且您也已經提供了 DCCloneConfig.xml，那麼新的 VM 會開機到目錄服務還原模式 (DSRM)。
    > -   為了提高 AD DS 服務的可用性，本指南建議並提供使用兩個不同 Hyper-V 主機的指示，以避免可能發生單一失敗點。 不過，您不需要兩個 Hyper-V 主機來執行虛擬網域控制站複製。
    > -   您必須是每個 Hyper-V 伺服器 (**HyperV1** 與 **HyperV2**) 上本機 Administrators 群組的成員。
    > -   為了使用 Hyper-V 順利地匯入和匯出 VHD 檔案，這兩個 Hyper-V 主機上的虛擬網路交換器應該是相同的名稱。 例如，如果 **HyperV1** 上有一個名為 VNet 的虛擬網路交換器，那麼 **HyperV2** 上的虛擬網路交換器名稱也應該是 VNet。
    > -   如果兩個 Hyper-V 主機 (**HyperV1** 與 **HyperV2**) 配備不同的處理器，請將想要匯出的虛擬機器 (**VirtualDC1**) 關機，在 VM 上按一下滑鼠右鍵，再依序按一下 [設定]、[處理器]，在 [處理器相容性] 之下選取 [移轉至使用不同處理器版本的實體電腦]，然後按一下 [確定]。

-   部署的 Windows Server 2012 網域控制站 （虛擬或實體） 裝載了 PDC 模擬器角色 (**DC1**)。 若要確認 Windows Server 2012 網域控制站上是否裝載 PDC 模擬器角色，請執行下列 Windows PowerShell 命令：

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    OperatingSystemVersion 值應該會傳回 6.2 版。 如有必要，您可以傳輸到執行 Windows Server 2012 網域控制站的 PDC 模擬器角色。 如需詳細資訊，請參閱 [使用 Ntdsutil.exe 傳輸或抓取 FSMO 角色到網域控制站](https://support.microsoft.com/kb/255504)。

-   已部署的 Windows Server 2012 來賓虛擬的網域控制站 (**VirtualDC1**) 與裝載 PDC 模擬器角色的 Windows Server 2012 網域控制站位於相同網域中 (**DC1**)。 這個會是用於複製的來源網域控制站。 來賓虛擬網域控制站會裝載在 Windows Server 2012 HYPER-V 伺服器 (**HyperV1**)。

    > [!NOTE]
    > -   若要順利複製，用來建立複製的來源網域控制站，不可以是上次建立來源 VHD 媒體之後被降級的 DC。
    > -   複製 VM 或其 VHD 之前，先將來源網域控制站關機。
    > -   要複製的 VHD 或要還原的快照，不可以早於標記存留期值 (如果啟用 Active Directory 資源回收筒，則不可以早於已刪除的物件存留期值)。 如果您正在複製現有網域控制站的 VHD，請確認 VHD 檔案沒有早於標記存留期值 (預設是 60 天)。 您不可以複製執行中網域控制站的 VHD 來建立複製媒體。

    退出來源 DC 上的任何虛擬軟碟機 (VFD)。 嘗試匯入新的 VM 時，可能會產生共用問題。

    只有 Windows Server 2012 網域控制站裝載在 Vm-generationid hypervisor 上可以用做為來源，進行複製。 用於複製的來源 Windows Server 2012 網域控制站應該處於狀況良好的狀態。 若要判斷來源網域控制站的狀態，請執行 [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx)。 若要深入了解 dcdiag 傳回的輸出，請參閱[用途 DCDIAG 實際...嗎?](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    如果來源網域控制站是 DNS 伺服器，則複製網域控制站也會是 DNS 伺服器。 您應該選用只裝載了整合 Active Directory 之區域的 DNS 伺服器。

    DNS 用戶端設定不會被複製，而是在 DCCloneConfig.xml 檔案中指定。 如果沒有指定這些設定，複製網域控制站預設會指向自己做為慣用 DNS 伺服器。 複製網域控制站將不會有 DNS 委派。 父系 DNS 區域的系統管理員應該視需要更新複製網域控制站的 DNS 委派。

    > [!WARNING]
    > 虛擬化保護並沒有涵蓋 Active Directory 輕量型目錄服務 (AD LDS)。 所以，請不要將 AD LDS 執行個體新增到 CustomDCCloneAllowList.xml，來嘗試複製裝載 AD LDS 執行個體的 AD DS 網域控制站。 因為 AD LDS 不具有 VM-Generation ID 感知，所以複製包含 AD LDS 的網域控制站，會導致該 AD LDS 組態集發生 USN 回復引發的不一致。

    下列伺服器角色不支援複製：

    -   動態主機設定通訊協定 (DHCP)

    -   Active Directory 憑證服務 (AD CS)

    -   Active Directory 輕量型目錄服務 (AD LDS)

### <a name="bkmk4_grant_source"></a>步驟 1:授與來源虛擬網域控制站可以被複製的權限
在這個程序中，您要使用 [Active Directory 管理中心]，將來源控制站新增到 [Cloneable Domain Controllers] 群組中，授與來源網域控制站可以被複製的權限。

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>授與來源虛擬網域控制站可以被複製的權限

1.  準備進行複製的網域控制站 (**VirtualDC1**) 期間，在位於相同網域的所有網域控制站上，開啟 [Active Directory 管理中心]  (ADAC)，找出虛擬網域控制站物件 (網域控制站通常位於 ADAC 的 [網域控制站]  容器之下)，用滑鼠右鍵按一下它，選擇 [加入群組]  ，並在 [請輸入物件名稱來選取]  之下輸入 **Cloneable Domain Controllers** ，然後按一下 [確定] 。

    這個步驟執行的群組成員資格更新必須複寫到 PDC 模擬器，才能執行複製。 如果**Cloneable Domain Controllers**找不到群組中，執行 Windows Server 2012 網域控制站上可能沒有裝載 PDC 模擬器角色。

    > [!NOTE]
    > 若要在 Windows Server 2012 網域控制站上開啟 ADAC，請開啟 Windows PowerShell 並輸入**dsac.exe**。

![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行的功能與前述程序相同：


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>步驟 2:執行 Get-ADDCCloningExcludedApplicationList Cmdlet
在這個程序中，請在來源虛擬網域控制站上執行 `Get-ADDCCloningExcludedApplicationList` Cmdlet，找出未評估是否可以複製的任何程式或服務。 您需要先執行 Get-ADDCCloningExcludedApplicationList Cmdlet 然後才執行 New-ADDCCloneConfigFile Cmdlet，因為如果 New-ADDCCloneConfigFile Cmdlet 偵測到未被包含在內的應用程式，它將不會建立 DCCloneConfig.xml 檔案。

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>找出來源網域控制站上執行的未被評估是否可以複製的應用程式或服務

1.  在來源網域控制站 (**VirtualDC1**) 上，依序按一下 [伺服器管理員]、[工具]、[Windows PowerShell 的 Active Directory 模組]，然後輸入下列命令：


    Get-ADDCCloningExcludedApplicationList


2.  與軟體廠商一起審視傳回的服務和已安裝程式的清單，判斷哪些可以被順利複製。 如果清單中的應用程式或服務不能被順利複製，就必須從來源網域控制站移除它們，否則複製會失敗。

3.  針對服務和已安裝的程式被判斷為可以順利複製，執行一次使用的指令 **"GenerateXML**切換至佈建這些服務和程式**CustomDCCloneAllowList.xml**檔案。


    Get-ADDCCloningExcludedApplicationList -GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>步驟 3:執行 New-ADDCCloneConfigFile
在來源網域控制站上執行 New-ADDCCloneConfigFile，並選擇性地指定複製網域控制站的組態設定，如名稱、IP 位址及 DNS 解讀器。

例如，若要建立名稱為 VirtualDC2 且具有靜態 IPv4 位址的複製網域控制站，請輸入：


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> 複製網域控制站會位於與來源網域控制站相同的站台中，除非在 DCCloneConfig.xml 檔案指定其他站台。 建議您在 DCCloneConfig.xml 檔案中按照複製網域控制站的 IP 位址，為它指定合適的站台。

電腦名稱可省略。 如果沒有指定電腦名稱，則會按照下列演算法產生唯一名稱：

-   首碼是來源網域控制站電腦名稱的前 8 個字元。 例如，來源電腦名稱 SourceComputer 會被截斷為 SourceCo 首碼字串。

-   格式的唯一命名尾碼""CL*nnnn*"會附加到首碼字串何處*nnnn*是從 0001-9999，PDC 判斷正在使用中的下一個可用值。 例如，如果 0047 是允許範圍內的下一個可用數字，使用前面的電腦名稱首碼 SourceCo 為例，會將複製電腦的衍生名稱設定為 SourceCo-CL0047。

> [!NOTE]
> New-ADDCCloneConfigFile Cmdlet 需要通用類別目錄伺服器 (GC) 才能順利運作。 中的來源網域控制站的成員資格**Cloneable Domain Controllers**群組，必須在 GC 反映出來。 GC 與 PDC 模擬器不需要是相同的網域控制站，但是最好位於相同的站台。 如果無法使用 GC，命令失敗並出現錯誤 「 伺服器 」 不操作。 如需詳細資訊，請參閱 [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。

若要建立名稱為 Clone1 且具有靜態 IPv4 設定的複製網域控制站，並指定慣用和替代 WINS 伺服器，請輸入：


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> 如果您指定 WINS 伺服器，您必須同時指定 **"PreferredWINSServer**並 **"AlternateWINSServer**。 如果只指定這些引數，複製會失敗，且 dcpromo.log 會出現錯誤碼 0x80041005。

若要建立名稱為 Clone2 且具有動態 IPv4 設定的複製網域控制站，請輸入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> 在這個情況中，環境中應該有一個複製網域控制站可以連線並取得 IP 位址及其他相關網路設定的 DHCP 伺服器。

若要建立名稱為 Clone2 且具有動態 IPv4 設定的複製網域控制站，並指定慣用和替代 WINS 伺服器，請輸入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


若要建立具有動態 IPv6 設定的複製網域控制站，請輸入：


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


若要建立具有靜態 IPv6 設定的複製網域控制站，請輸入：


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> 指定 IPv6 設定時，靜態與動態之間唯一的差別是包含 **-靜態**切換。 包含 **-靜態**參數會強制指定至少一個**IPv6DNSResolver**。若要設定透過無狀態位址自動設定 (SLAAC) 且路由器指派首碼預期靜態 IPv6 位址。 使用動態 IPv6，DNS 解析程式是選擇性，但預期的複製品，可以連線到啟用 IPv6 的 DHCP 伺服器取得 IPv6 位址和 DNS 設定資訊的子網路上。

#### <a name="BKMK_OfflineMode"></a>在離線模式中執行 New-addccloneconfigfile
如果您有多個已經準備用於複製之來源網域控制站的媒體複本 (意指來源網域控制站具備複製的權限、已經執行過 Get-ADDCCloningExcludedApplicationList Cmdlet 等)，而且您想要為每個媒體複本指定不同的設定，那麼可以在離線模式中執行 New-ADDCCloneConfigFile。 這比個別準備 VM (例如，匯入每個複本) 的方式來得有效率。

在此情況下，網域系統管理員可以掛載離線磁碟，並以新增 XML 檔案，以達到原廠相似的自動化使用 new-offline 引數與使用遠端伺服器管理工具 (RSAT) 執行 New-addccloneconfigfile cmdlet內含於 Windows Server 2012 的 Windows PowerShell 選項。 如需如何掛載離線磁碟以便在離線模式中執行 New-ADDCCloneConfigFile Cmdlet 的詳細資訊，請參閱[將 XML 新增到離線系統磁碟](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline)。

您應該先在來源媒體上從本機執行 Cmdlet，確保通過先決條件檢查。 先決條件檢查不能在離線模式下執行，因為 Cmdlet 可能會從不是位於相同網域的電腦或不是從加入網域的電腦執行。 在本機執行 Cmdlet 之後，它會建立一個 DCCloneConfig.xml 檔案。 如果您打算之後繼續使用離線模式，可以刪除這個在本機建立的 DCCloneConfig.xml。

若要建立的複製網域控制站名為 CloneDC1 在離線模式中，呼叫 REDMOND 網站中 「 靜態 IPv4 位址類型：


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


若要在離線模式中建立名稱為 Clone2 且具有靜態 IPv4 和靜態 IPv6 設定的複製網域控制站，請輸入：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


若要在離線模式中建立具有靜態 IPv4 和動態 IPv6 設定的複製網域控制站，並為 DNS 解讀器設定指定多個 DNS 伺服器，請輸入：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


若要在離線模式中建立名稱為 Clone1 且具有動態 IPv4 和靜態 IPv6 設定的複製網域控制站，請輸入：


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


若要在離線模式中建立具有動態 IPv4 和動態 IPv6 設定的複製網域控制站，請輸入：


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>步驟 4:匯出再匯入來源網域控制站的虛擬機器
在這個程序中，您要匯出來源虛擬網域控制站的虛擬機器，然後再匯入虛擬機器。 這個動作會在您的網域中建立一個複製虛擬網域控制站。

您必須是每個 Hyper-V 主機上本機 Administrators 群組的成員。 如果您為每個伺服器使用不同的認證，請在不同的 Windows PowerShell 工作階段中執行 Windows PowerShell Cmdlet 來匯出和匯入 VM。

如果來源網域控制站上有快照，您必須先刪除這些快照才匯出來源網域控制站，因為如果快照中包含與目標 Hyper-V 主機不相容的處理器設定，就無法匯入 VM。 如果來源和目標 Hyper-V 主機之間的處理器設定是相容的，就可以匯出和複製來源，而不用事先刪除這些快照。 不過，在匯入之後，必須先刪除複製 VM 的快照才能啟動。

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>匯出再匯入虛擬來源網域控制站來複製虛擬網域控制站

1.  在 **HyperV1** 上，將來源網域控制站 (**VirtualDC1**) 關機。

    ![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

    停止 VM-命名 VirtualDC1-ComputerName HyperV1


2.  在 **HyperV1** 上，刪除快照，然後將來源網域控制站 (VirtualDC1) 匯出到 c:\CloneDCs 目錄。

> [!NOTE]
> 您必須刪除所有關聯的快照，因為每次建立快照時，就會建立一個新的 AVHD 檔案做為區別的磁碟。 這樣會引起連鎖影響。 如果您已經建立快照並將 DCCLoneConfig.xml 檔案插入到 VHD，可能會從較舊的 DIT 版本建立複製，或是將設定檔插入到錯誤 VHD 檔案。 刪除快照會將所有這些 AVHD 合併到基底 VHD。

![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  將 **virtualdc1** 資料夾複製到 **HyperV2** 的 c:\Import 目錄。

4.  在 **HyperV2**上，使用 [Hyper-V 管理員] 從 **c:\Import\virtualdc1** 資料夾匯入虛擬機器 (使用 [Hyper-V 管理員] 中的 [匯入虛擬機器]  精靈)，並刪除所有關聯的 [快照] 。

匯入虛擬機器時，請使用 [複製虛擬機器 (建立新的唯一識別碼)] 選項。

![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


若要從相同的來源網域控制站建立多個複製網域控制站：

  -   UI：在 [匯入虛擬機器]  精靈中，為 [虛擬機器設定資料夾] 、[快照存放區] 、[智慧型分頁資料夾] 指定新的位置，為虛擬機器的虛擬硬碟的 [位置]  指定不同的位置。

  -   Windows PowerShell： 使用下列參數，為指定的虛擬機器的新位置`Import-VM`cmdlet:

        $path = Get-childitem 「 C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual 機器 」 IMPORT-VM-路徑 $path.fullname-複製-GenerateNewId-ComputerName HyperV2-VhdDestinationPath"path"-SnapshotFilePath"path"-SmartPagingFilePath 「 路徑 」-VirtualMachinePath 「 路徑 」


> [!NOTE]
> 同時建立多個複製網域控制站的建議批次大小為 10。 最大數受到連出複寫連線最大數的限制；分散式檔案系統複寫 (DFSR) 的預設值是 16，檔案複寫服務 (FRS) 的預設值是 10。 同時部署的複製網域控制站數目不應該超過建議的數目，除非您已經在您的環境中完整地測試過。

5.  在 **HyperV1**上，重新啟動來源網域控制站 (**VirtualDC1**)，讓它重新連線。

![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  在 **HyperV2**上，啟動虛擬機器 (**VirtualDC2**)，讓它連線成為網域中的複製網域控制站。

![AD DS 的簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> 必須執行 PDC 模擬器，複製才會成功。 如果關閉 PDC 模擬器，請確認它已啟動並執行初始同步化，所以它會感知到它持有 PDC 模擬器角色。 如需詳細資訊，請參閱 Microsoft [KB 文章 305476](https://support.microsoft.com/kb/305476)。

複製完成之後，驗證複製電腦的名稱，確保複製作業成功。 確認 VM 沒有以目錄服務還原模式 (DSRM) 啟動。 如果您嘗試登入並收到指出沒有可用登入伺服器的錯誤訊息，請試著在 DSRM 模式中登入。 如果 DC 沒有順利複製而且以 DSRM 模式開機，請查看事件檢視器中的記錄檔以及 %systemroot%/debug 資料夾中的 dcpromo 記錄檔。

複製網域控制站會是 [Cloneable Domain Controllers] 群組的成員，因為它從來源網域控制站複製了成員資格。 最佳做法是讓 [Cloneable Domain Controllers] 群組保留空白，直到您準備好執行複製作業，而且複製作業完成之後應該要移除成員。

如果來源網域控制站儲存了備份媒體，複製網域控制站也會儲存備份媒體。 您可以執行 `wbadmin get versions` ，顯示複製網域控制站上的備份媒體。 Domain Admins 群組的成員應該刪除複製網域控制站上的備份媒體，防止它不小心被還原。 如需如何使用 wbadmin.exe 刪除系統狀態備份的詳細資訊，請參閱 [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx)。

## <a name="troubleshooting"></a>疑難排解
如果複製網域控制站 (**VirtualDC2**) 以目錄服務還原模式 (DSRM) 啟動，則下次開機時它無法自行回到正常模式。 若要登入以 DSRM 啟動的網域控制站，請使用 **.\Administrator** 並指定 DSRM 密碼。

修正造成複製失敗的原因，確認 dcpromo.log 沒有指出無法重試複製。 如果無法重試複製，請以安全的方式捨棄媒體。 如果可以重試複製，您必須移除 DS 還原模式開機旗標，才能重試複製。

1.  開啟 Windows Server 2012 （右側按一下 Windows Server 2012 並選擇 以系統管理員身分執行），提升權限的命令，然後鍵入**msconfig**。

2.  在 [開機]  索引標籤的 [開機選項] 之下，清除 [安全開機]  (如果 [Active Directory 修復] 選項啟用，則已被選取)。

3.  按一下 [確定]  ，在出現提示時重新啟動。

如需虛擬網域控制站疑難排解的詳細資訊，請參閱[虛擬網域控制站疑難排解](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。


