---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: "Active Directory Domain Services (AD DS) 模擬 (層級 100) 簡介"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3c7fdc2e20bc4a27f2555af54f396a15f664d6c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Active Directory Domain Services (AD DS) 模擬 (層級 100) 簡介

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services (AD DS) 環境模擬已執行許多年。 開始使用 Windows Server 2012，AD DS 提供更大虛擬網域控制站化簡介模擬安全功能，並讓快速部署透過複製 virtual 網域控制站的支援。 這些新模擬功能提供更多支援的公開和私人雲朵、混合的環境 AD DS 部分存在先和完全位於 AD DS 基礎結構和雲端，先。

**本文件**

-   [安全 virtualization 網域控制站的](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#safe_virt_dc)

-   [模擬的網域控制站複製](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#virtualized_dc_cloning)

-   [步驟部署複製擬化檔案網域控制站](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)

-   [疑難排解](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#troubleshooting)

## <a name="safe_virt_dc"></a>安全 virtualization 網域控制站的
Virtual 環境獨特的挑戰到分散式工作負載的邏輯時鐘複寫配置而定。 AD DS 複寫，例如使用單純地增加值（稱為 USN 或更新的序號）指派給在每個網域控制站交易。 每個網域控制站資料庫執行個體的身分，稱為「呼叫識別碼也提供。 呼叫識別碼網域控制站和其 USN 它們會被與每個寫入交易相關聯的唯一每個網域控制站上執行，必須樹系的唯一。

AD DS 複寫使用呼叫識別碼和 Usn 在每個網域控制站判斷需要複製到其他網域控制站的變更。 如果網域控制站復原時間以外的網域控制站感知 USN 重複使用完全不同交易，複寫將會減少因為其他網域控制站將它們有收到已經在該呼叫識別碼部分重複使用 USN 相關聯的更新。

例如下圖顯示，就會發生事件的順序 Windows Server 2008 R2 和更早版本作業系統上 VDC2，一樣所執行的目的地網域控制站偵測到 USN 復原時。 在此範例中，USN 復原偵測時發生上 VDC2 複寫合作夥伴偵測 VDC2 已送出先前已過複寫合作夥伴，表示該 VDC2 的資料庫有復原的時間不正確的最新 USN 值。

![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

一樣 (VM) 可讓您輕鬆 hypervisor 復原網域控制站的 Usn（邏輯時鐘），系統管理員，例如網域控制站的感知以外的快照。 如需有關 USN 和 USN 回復，包括另一個圖示範無法偵測執行個體 USN 復原，請[USN 和 USN 復原](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback)。

開始使用 Windows Server 2012，AD DS virtual 網域控制站公開識別字稱為 VM-Generation ID hypervisor 平台上可以偵測及使用如果一樣復原時間 VM 快照的應用程式來保護 AD DS 環境的所需的安全性措施。 VM-GenerationID 設計公開的地址空間來賓一樣，在此識別碼安全模擬體驗一致提供的任何 hypervisor 支援 VM-GenerationID，以便使用 hypervisor 廠商獨立機制。 可以將此識別碼取樣服務及一樣中執行的應用程式來偵測如果一樣已復原時間。

### <a name="BKMK_HowSafeguardsWork"></a>這些模擬安全防護如何運作？
網域控制站在安裝期間，AD DS 最初儲存 VM GenerationID 識別碼 msDS-GenerationID 屬性它（通常稱為樹狀資訊或 DIT）的資料庫中的網域控制站電腦物件的一部分。 VM GenerationID 會獨立追蹤 inside 一樣的 Windows 驅動程式。

當系統管理員從先前的快照還原一樣時，目前的驅動程式一樣 VM GenerationID 值比較 DIT 中的值。

如果有兩個值不同，呼叫識別碼重設並 RID 集區捨棄重複使用藉此阻止 USN。 如果是相同的值，交易交付正常。

AD DS 也會比較 VM GenerationID 一樣對 DIT 中值從目前的值每次的網域控制站重新開機，如果不同，它會重設呼叫識別碼，會捨棄 RID 集區的更新和 DIT 使用新的值。 它也非-系統授權同步 SYSVOL 資料夾時間才能完成安全還原。 這可讓延伸的快照，在 Vm 中，關閉應用程式防護。 Windows Server 2012 中引進了這些安全防護讓 AD DS 系統管理員受益部署及管理網域控制站模擬的環境中唯一的優點。

下圖顯示了相同的 USN 復原偵測到模擬的網域控制站支援 VM-GenerationID hypervisor 上執行 Windows Server 2012 時，如何套用模擬防護功能。

![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

若是如此，當 hypervisor 偵測到 VM-GenerationID 值來變更，觸發模擬防護功能，包括針對（從 A 到先前的範例中為 B) 模擬網域控制站的呼叫識別碼重設並 VM 符合 hypervisor 儲存新值 (G2) 上更新 VM-GenerationID 值儲存。 保護確保複寫會聚兩個網域控制站的合成。

與 Windows Server 2012，AD DS 受到保護 virtual 網域控制站 VM-GenerationID 注意 hypervisors 上，可確保意外應用程式的快照或其他這類 hypervisor 式機制，無法復原一樣的狀態不會中斷 AD DS 環境（由避免複寫問題，例如 USN 泡泡或延遲物件）。 不過，還原網域控制站快照一樣，建議您不要做為備份的網域控制站替代機制。 建議您繼續使用 Windows Server 備份或其他 VSS writer 備份方案。

> [!CAUTION]
> 如果網域控制站在 production 環境不小心會還原成快照，建議您的應用程式，請洽詢供應商，並還原服務位於該一樣，指導方針之後快照確認這些程式的狀態。

如需詳細資訊，請查看[擬化檔案網域控制站安全還原架構](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。

## <a name="virtualized_dc_cloning"></a>模擬的網域控制站複製
開始使用 Windows Server 2012，系統管理員可以輕鬆地部署複本網域控制站複製現有 virtual 網域控制站。 在 virtual 環境中，系統管理員不再需要重複部署準備好使用 sysprep.exe 伺服器影像、 為網域控制站伺服器升級，然後完成部署每個複本網域控制站的需求額外的設定。

> [!NOTE]
> 系統管理員必須遵循部署，例如使用 sysprep.exe 準備伺服器 virtual 硬碟 (VHD)、 為網域控制站伺服器升級，然後完成任何額外的設定需求網域中的第一個網域控制站現有處理程序。 在損壞復原案例中，使用最新的伺服器備份還原網域中的第一個網域控制站。

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>受惠於 virtual 網域控制站複製案例

-   快速部署新的網域中的其他網域控制站

-   快速期間損壞修復還原透過快速部署使用複製的網域控制站 AD DS 容量還原業務持續性

-   利用彈性提供的網域控制站容納提升的縮放需求最佳化部署私人雲端

-   迅速提供讓部署及測試的新功能推出 production 之前測試環境

-   快速符合更高的容量，複製現有的網域控制站在分公司分公司需求

快速部署大量的網域控制站時, 繼續依照驗證每個網域控制站的健康狀態，在安裝完成後現有的程序。 部署網域控制站合理大小分批，因此每一批的安裝完成後，您就可以驗證他們健康。 建議批次大小為 10。 如需詳細資訊，請查看[步驟部署複製模擬的網域控制站的](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)。

### <a name="clear-separation-of-responsibilities"></a>清除區隔責任
複製模擬的網域控制站的授權可在 AD DS 系統管理員所控制。 為了讓其他網域控制站部署複製 virtual 網域控制站 hypervisor 系統管理員，AD DS 系統管理員必須選取及授權網域控制站準備要做為來源複製的步驟執行。

使用一樣提供通常會在 purview hypervisor 系統管理員，hypervisor 系統管理員可以複製模擬的網域控制站的授權，並由 AD DS 系統管理員可以複製準備提供複本網域控制站虛擬電腦。

> [!WARNING]
> 任何人都可以管理主控 virtual 網域控制站 hypervisor 必須高度信任並稽核環境中。

### <a name="how-does-virtual-domain-controller-cloning-work"></a>複製工作 virtual 網域控制站如何？
複製的程序，包括讓複製現有 virtual 網域控制站的 VHD 的 （或，更複雜的設定，VM 網域控制站），它複製 AD ds 和建立設定檔複製授權。 這樣可以降低數個步驟，而且時間參與，否則排除部署複本 virtual 網域控制站重複部署工作。

複製網域控制站偵測是另一個網域控制站的複本，使用下列條件：

1.  新一代 VM ID 一樣所提供的值為不同於 VM 新一代 ID DIT 中儲存的值。

    > [!NOTE]
    > Hypervisor 平台必須支援 VM 新一代 ID （Windows Server 2012 HYPER-V 支援 VM 新一代 ID）。

2.  有某個檔案稱為 DCCloneConfig.xml 在下列位置：

    -   所在 DIT directory

    -   %windir%\NTDS

    -   卸除式媒體磁碟機的根

一旦符合的條件，它會複製到為網域控制站複本本身規定的程序。

複製網域控制站使用來源網域控制站 （網域控制站它所代表的複本） 的安全性層級連絡 Windows Server 2012 主要網域控制站 (PDC) 模擬器作業主角持有 （也稱為彈性的單一主機操作或 FSMO）。 肯定必須執行 Windows Server 2012，但不需要在 hypervisor 上執行。

> [!NOTE]
> 如果您擁有的參考來源網域控制站的屬性架構延伸模組，而且屬性的其中一個物件複製電腦物件 （NTDS 設定物件） 來建立複製，該屬性會不複製或參考複製網域控制站的更新。

確認要求網域控制站的複製的授權之後, 肯定將建立新的電腦身分包括新 account、 SID、 名稱及辨識為網域控制站複本這台電腦的密碼，並回到複製傳送此資訊。 複製網域控制站然後將會準備 AD DS 資料庫做為複本檔案，以及它也會清除的電腦狀態。

如需詳細資訊，請查看[Virtualized 網域控制站複製架構](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)。

### <a name="cloning-components"></a>複製元件
新 cmdlet 納入 Active Directory 模組中相關的 XML 檔案的 Windows PowerShell 複製元件：

-   **新 ADDCCloneConfigFile** 「 下列 cmdlet 建立並地點 DCCloneConfig.xml，以確保已觸發複製正確的位置。 它也會執行必要條件檢查以確保成功複製。 它適用於 Windows PowerShell 包含在 Active Directory 模組。 您可以執行本機模擬的網域控制站的已準備好可以複製，或您可以從遠端使用地執行-離線選項。 您可以指定複製網域控制站，例如其名稱、 網站及 IP 位址設定。

    它會執行的必要條件檢查︰

    > [!NOTE]
    > 必要條件檢查不是執行時 」 使用離線選項。 如需詳細資訊，請查看[執行新-ADDCCloneConfigFile 離線模式在](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode)。

    -   正在準備 DC 授權的複製 (成員的**複製網域控制站**群組)

    -   肯定執行 Windows Server 2012。

    -   任何程式或服務列執行的**取得-ADDCCloningExcludedApplicationList** （此清單複製元件結尾處的更多詳細資料所述） CustomDCCloneAllowList.xml 中包含。

-   **DCCloneConfig.xml** 「 成功複製模擬的網域控制站，此檔案必須是 directory DIT 所在位置，在*%windir%\NTDS*，或根本卸除式媒體磁碟機。 除了用於發射鍵的其中一個做為偵測及起始複製，也提供一種方法來指定複製網域控制站的設定。

    架構和範例檔案 DCCloneConfig.xml 檔案會儲存在所有 Windows Server 2012 電腦上：

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    建議您使用新的 ADDCCloneConfigFile cmdlet 建立 DCCloneConfig.xml 檔案。 您也可以使用 XML 感知編輯器使用架構檔案來建立該檔案，但手動編輯檔案增加錯誤的機會。 如果您要編輯檔案，必須完成使用 XML 感知編輯器，Visual Studio 中，例如[XML 記事本](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)，或第三方應用程式 （無法使用 「 記事本 」）。

-   **取得-ADDCCloningExcludedApplicationList** 「 來源網域控制站之後，再開始複製程序，以判斷哪一個服務或安裝的程式無法在預設支援清單中，DefaultDCCloneAllowList.xml，執行下列 cmdlet 或使用者定義包含清單命名的 CustomDCCloneAllowList.xml 的檔案，因此不評估複製影響的。

    這個 cmdlet 搜尋服務的來源網域控制站在服務控制管理員中，並已安裝的程式會列在**HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** ，預設清單 (DefaultDCCloneAllowList.xml) 中未指定或，如果有一個提供，使用者定義包含清單 （CustomDCCloneAllowList.xml 檔案）。 清單中的應用程式與服務執行 cmdlet 傳回有不同功能已提供 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 檔案隨即執行階段依據來源俠上已安裝的項目清單中。 如果您判斷該程式與服務可以放心地複製可以取得-ADDCCloningExcludedApplicationList 的服務和程式輸出新增 CustomDCCloneAllowList.xml 檔案。 若要判斷是否可以放心地複製服務或安裝程式，評估下列條件：

    -   為電腦的身分，例如名稱、 SID、 密碼，所受到的安裝的程式或服務？

    -   不會服務，或在本機可能會影響複製上的功能的電腦上安裝程式儲存區任何狀態？

    您必須使用與軟體廠商的應用程式，判斷是否服務或程式可以放心地複製。

    > [!NOTE]
    > 之前，提供額外的服務或 CustomDCCloneAllowList.xml 檔案中的程式，請先確認您是否擁有所需的授權複製該一樣上的該軟體。

    如果不複製應用程式，請移除來源網域控制站的之前建立複製媒體。 如果應用程式在 cmdlet 輸出中，會顯示，但不是會納入 CustomDCCloneAllowList.xml 檔案，將會失敗複製。 複製才能繼續，cmdlet 輸出不應該列出程式或服務。 亦即，應用程式必須會包含在 CustomDCCloneAllowList.xml 檔案或移除來源網域控制站。

    下表解釋執行取得-ADDCCloningExcludedApplicationList 的選項。

    |||
    |-|-|
    |引數|解釋|
    |*<no argument specified>*|不已達到複製的主機上顯示服務的程式清單。 如果已經有任何允許位置 CustomDCCloneAllowList.XML，它會使用的檔案以顯示的剩餘服務和程式 （如果清單符合可能會執行任何動作）。|
    |-GenerateXml|建立的填入服務 CustomDCCloneAllowList.XML 檔案和主機上所列的程式。|
    |-推動|覆寫現有的 CustomDCCloneAllowList.XML 檔案。|
    |路徑|若要建立 CustomDCCloneAllowList.XML 資料夾路徑。|

-   **DefaultDCCloneAllowList.xml** 「 這個檔案已存在預設每個 Windows Server 2012 網域控制站在*%windir%\system32*。 它會列出的服務，並安裝的程式可以放心地複製預設。 您必須變更的位置或檔案的內容或複製將會失敗。

-   **CustomDCCloneAllowList.xml** 「 您有服務或安裝的程式位於來源網域控制站以外，這些 DefaultDCCloneAllowList.xml 檔案中列出的如果那些服務和程式必須包含此檔案中。 若要尋找的服務或安裝的程式，不會列在 DefaultDCCloneAllowList.xml 檔案，在執行**取得-ADDCCloningExcludedApplicationList** cmdlet。 您應該使用**「 GenerateXml**引數建立 XML 檔案。

    複製程序會檢查以下位置訂單此檔案中的，並使用找到，無論其他資料夾的第一個 XML 檔案：

    1.  下列機碼：

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  使用 Directory DSA

    3.  %systemroot%\NTDS

    4.  讀取/寫入卸除式媒體，在磁碟機代號，在磁碟機的根的訂單

### <a name="deployment-scenarios"></a>部署案例
下列部署案例的支援 virtual 網域控制站複製：

-   部署複製網域控制站藉由來源網域控制站的 virtual 硬碟 (vhd) 檔案的複本。

-   部署複製網域控制站複製使用/匯出語意 hypervisor 公開的來源網域控制站的一樣。

> [!NOTE]
> 一節中的步驟執行[步驟部署複製模擬的網域控制站的](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)示範複製使用匯出日匯入功能的 Windows Server 2012 HYPER-V 一樣。

## <a name="steps_deploy_vdc"></a>步驟部署複製擬化檔案網域控制站

-   [必要條件](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [步驟 1： 來源模擬的網域控制站權限授與複製](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [步驟 2： 執行取得-ADDCCloningExcludedApplicationList cmdlet](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [步驟 3： 執行新 ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [步驟 4： 匯出與然後匯入的來源網域控制站一樣](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>必要條件

-   若要完成下列程序中的步驟，您必須網域管理群組成員或相同的權限指派給它。

-   本指南使用的 Windows PowerShell 命令必須從提升權限的命令提示字元中執行。 若要這樣做，請以滑鼠右鍵按一下**Windows PowerShell**圖示，然後再按一下**以系統管理員身分執行**。

-   Windows Server 2012 伺服器安裝 HYPER-V 伺服器角色 (**HyperV1**)。

-   Windows Server 2012 第二個安裝 HYPER-V 伺服器角色伺服器 (**HyperV2**)。

    > [!NOTE]
    > -   如果您使用其他 hypervisor，您應該連絡該 hypervisor 廠商，以確認是否 hypervisor 支援 VM 新一代 id。 如果 hypervisor 不支援 VM 新一代 ID，您所提供的 DCCloneConfig.xml 新 VM 會開機至 Directory 服務還原模式 (DSRM)。
    > -   若要增加 AD DS 服務的可用性、 本指南建議您，並提供使用兩個不同的 HYPER-V 主機，有助於避免潛在一點失敗的指示操作。 不過，您不需要執行 virtual 網域控制站複製兩個 HYPER-V 主機。
    > -   您必須在每個 HYPER-V server 本機系統管理員群組成員 (**HyperV1**和**HyperV2**)。
    > -   成功匯入及匯出使用 HYPER-V VHD 檔案，以便在兩個 HYPER-V 主機 virtual 網路參數應該會有相同的名稱。 例如，如果您有 virtual 網路開機**HyperV1**然後需要有 virtual 網路開機名 VNet **HyperV2**名 VNet。
    > -   如果有兩個 HYPER-V 主機 (**HyperV1**和**HyperV2**) 有不同的處理器、 關機一樣 (**VirtualDC1**) 想要匯出，VM 上按一下滑鼠右鍵按一下**設定**，按一下 [**處理器**，並在**處理器的相容性**選取**移轉實體處理器不同版本的電腦**，按一下 [ **[確定]**。

-   部署的 Windows Server 2012 裝載網域控制站 （模擬或實體） PDC 模擬器角色 (**DC1**)。 若要確認是否 PDC 模擬器角色裝載網域控制站 Windows Server 2012 上，執行下列 Windows PowerShell 命令：

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    OperatingSystemVersion 值應為版本 6.2 傳回。 必要時，您可以執行 Windows Server 2012 」 的網域控制站傳輸 PDC 模擬器角色。 如需詳細資訊，請查看[使用傳輸或抓取故障網域控制站的 Ntdsutil.exe](https://support.microsoft.com/kb/255504)。

-   部署的 Windows Server 2012 來賓模擬的網域控制站 (**VirtualDC1**)，位於與 Windows Server 2012 網域控制站裝載 PDC 模擬器角色相同的網域 (**DC1**)。 這將會使用複製的來源網域控制站。 將會在 Windows Server 2012 HYPER-V server 裝載來賓 virtual 網域控制站 (**HyperV1**)。

    > [!NOTE]
    > -   複製才能繼續，用來建立複製來源網域控制站不能從因為來源 VHD 媒體建立降級 DC。
    > -   關閉之前複製 VM 或其 VHD 來源網域控制站。
    > -   您不應該複製 VHD 或還原標記期間值 （或 Active Directory 資源回收桶支援的刪除的物件期間值） 較舊的快照。 如果您要複製的現有的網域控制站 VHD，請務必 VHD 檔案不是較舊的標記期間值 （預設 60 天）。 您不應該複製執行網域控制站 VHD 建立複製媒體。

    退出 (VFD) 來源俠可能會有任何 virtual 磁碟機。 嘗試新 VM 匯入時，這會造成共用的問題。

    僅限 Windows Server 2012 網域控制站 VM-GenerationID hypervisor 上可做為來源的複製。 使用複製的來源 Windows Server 2012 網域控制站應該健全狀態。 若要判斷的執行來源網域控制站狀態[dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx)。 若要獲得更好了解傳回 dcdiag 的輸出中，查看[的功能 DCDIAG 確實...？](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    如果來源網域控制站的 DNS 伺服器，複製的網域控制站也會 DNS 伺服器。 您應該選擇該主機只 Active Directory 整合區域 DNS 伺服器。

    不會複製 DNS client 設定，但改為詳列於 DCCloneConfig.xml 檔案。 如果未指定其，複製的網域控制站將指向本身為慣用 DNS 伺服器預設。 複製的網域控制站不會有 DNS 委派。 家長 DNS 區域的系統管理員應該更新視需要複製的網域控制站 DNS 委派。

    > [!WARNING]
    > Active Directory 輕量型 Directory services (AD LDS) 延伸模擬防護功能。 因此您應該嘗試複製 AD DS 網域控制站 AD LDS 焦加入 CustomDCCloneAllowList.xml 裝載的廣告 LDS 執行個體。 廣告 LDS 不是 VM 新一代 ID 注意，因為複製網域控制站的廣告 LDS 會導致 USN 復原引入分歧該 AD LDS 組態的設定。

    不支援下列伺服器角色複製：

    -   動態主機設定通訊協定」(DHCP)

    -   Active Directory 憑證 Services (AD CS)

    -   Active Directory 輕量 Directory Services (AD LDS)

### <a name="bkmk4_grant_source"></a>步驟 1： 來源模擬的網域控制站權限授與複製
此程序，在您授與來源網域控制站的權限複製使用**Active Directory 管理中心**來新增來源網域控制站**複製網域控制站**群組。

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>若要複製的權限授與來源模擬的網域控制站

1.  正在準備複製的網域控制站相同的網域中的任何網域控制站 (**VirtualDC1**)，開放**Active Directory 管理中心**(ADAC)，找不到模擬的網域控制站物件 (通常位於網域控制站在**網域控制站**中 ADAC 容器)，以滑鼠右鍵按一下它，選擇**新增到群組**和在**輸入物件名稱來選取**輸入**複製網域控制站**，然後按一下 [ **[確定]**。

    在此步驟執行群組成員資格更新必須複製到肯定複製才能執行。 如果**的網域控制站複製**找不到群組，可能不會在執行 Windows Server 2012 」 的網域控制站裝載 PDC 模擬器角色。

    > [!NOTE]
    > Windows Server 2012 網域控制站打開 ADAC，開放的 Windows PowerShell 並輸入**dsac.exe**。

![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能：


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>步驟 2： 執行取得-ADDCCloningExcludedApplicationList cmdlet
此程序，在執行`Get-ADDCCloningExcludedApplicationList`上找出 [所有程式或服務，不會評估複製的來源模擬的網域控制站 cmdlet。 您需要執行取得-ADDCCloningExcludedApplicationList cmdlet 新-ADDCCloneConfigFile cmdlet 之前，因為如果新-ADDCCloneConfigFile cmdlet 偵測到排除的應用程式，就不會建立 DCCloneConfig.xml 檔案。

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>找出應用程式或服務執行來源網域控制站的複製尚未評估

1.  來源網域控制站上 (**VirtualDC1**)，按一下 [**伺服器管理員**，按一下 [**工具**，按一下**Active Directory 模組適用於 Windows PowerShell** ，然後輸入下列命令：


    取得-ADDCCloningExcludedApplicationList


2.  若要判斷是否他們可以放心地複製軟體廠商對傳回的服務和已安裝的程式清單。 如果應用程式或服務的清單中找不安全複製，您必須移除來源網域控制站或複製將會失敗。

3.  一組服務，並判斷安全地複製已安裝的程式，執行一次使用命令**「 GenerateXML**切換提供這些服務和程式中**CustomDCCloneAllowList.xml**檔案。


    取得-ADDCCloningExcludedApplicationList-GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>步驟 3： 執行新 ADDCCloneConfigFile
ADDCCloneConfigFile 新的執行來源網域控制站並選擇指定名稱、 IP 位址和 DNS 解析複製網域控制站的設定。

例如，以建立複製網域控制站名 VirtualDC2 靜態 IPv4 位址，請輸入：


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> 複製網域控制站會位於相同的來源網域控制站網站除非 DCCloneConfig.xml 檔案中指定不同的網站。 建議您在根據其 IP 位址複製網域控制站 DCCloneConfig.xml 檔案中指定適當的網站。

電腦名稱是選擇性的。 如果您不指定一個，將根據下列演算法產生唯一名稱：

-   前置詞是來源網域控制站的電腦名稱的第一次 8 個字元。 例如，SourceComputer 的來源電腦的名稱會被截斷 SourceCo 為前置詞字串。

-   格式的唯一命名尾碼 」 」 CL*nnnn*」 附加至前置詞字串其中* nnnn *是從 0001 9999 PDC 判斷正在使用中的下一個可用值。 例如，0047 是否允許的範圍中的下一步使用數字，使用先前的電腦名稱前置詞 SourceCo，範例衍生使用複製的電腦名稱將會設定為 SourceCo-CL0047。

> [!NOTE]
> 新增-ADDCCloneConfigFile cmdlet 順利運作的必要通用伺服器 (GC)。 來源網域控制站的成員資格**的網域控制站複製**群組必須會反映出剛剛在 GC。 不需要肯定，以相同的網域控制站 GC，但最好它應該會在相同的網站。 如果無法使用 GC、 命令失敗，錯誤 」 伺服器是不作業 」。 如需詳細資訊，請查看[擬化檔案網域控制站疑難排解](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。

若要建立的靜態 IPv4 設定名 Clone1 複製網域控制站指定慣用及其他 WINS 伺服器，鍵入：


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> 若您指定 WINS 伺服器，您必須指定兩者**「 PreferredWINSServer**和**」 AlternateWINSServer**。 如果您只這些引數指定複製失敗，錯誤代碼 0x80041005 dcpromo.log 中出現。

若要建立複製網域控制站名 Clone2 動態 IPv4 設定，請輸入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> 若是如此，應該會有 DHCP 伺服器環境複製可以瑞曲之戰並取得 IP 位址和其他相關的網路設定中。

若要建立名 Clone2 動態 IPv4 設定的網域控制站複製指定慣用及其他 WINS 伺服器，鍵入：


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


若要建立複製網域控制站的動態 IPv6 設定，請輸入：


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


若要建立複製網域控制站的靜態 IPv6 設定，請輸入：


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> 指定 IPv6 設定時, 僅限靜態和動態設定不同的是包含**-靜態**切換。 包含**-靜態**選項可讓您管轄中指定至少一個**IPv6DNSResolver**。無狀態地址自動設定 (SLAAC) 路由器指派前置詞透過預期靜態 IPv6 位址。 動態 IPv6，使用 DNS 解析程式是選擇性的但預期複製可以瑞曲之戰 IPv6 式上 DHCP 伺服器子網路，以取得 IPv6 位址和 DNS 設定的資訊。

#### <a name="BKMK_OfflineMode"></a>離線模式中的執行新 ADDCCloneConfigFile
如果您有多個複本已經準備複製 （來源網域控制站的複製授權，取得-ADDCCloningExcludedApplicationList cmdlet 已，這表示執行，等等） 來源網域控制站媒體，並想要的每一份媒體不同設定，您可以執行新-ADDCCloneConfigFile 離線模式。 這可能是比排列匯入的每個複本以準備每個 VM，例如更有效率。

在這種情形下，網域系統管理員可以雷離線磁碟和使用遠端伺服器管理工具 (RSAT) 執行新-ADDCCloneConfigFile cmdlet-離線引數以新增 XML 檔案，可以針對原廠類似的自動化使用新的 Windows PowerShell 選項包含 Windows Server 2012 中。 如需了解如何以執行離線模式中的新-ADDCCloneConfigFile cmdlet 雷離線磁碟的詳細資訊，請查看[新增離線系統磁碟的 XML](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline)。

您應該先 cmdlet 本機上執行以確定該必要條件檢查 pass 來源媒體。 因為您的電腦可能無法從相同的網域或加入網域的電腦無法執行 cmdlet 必要條件檢查不會執行離線模式。 您在本機上執行 cmdlet 之後，它將會建立 DCCloneConfig.xml 檔案。 您可能會 delete 建立本機如果您打算使用離線模式後續 DCCloneConfig.xml。

若要建立網域控制站複製名為 CloneDC1 離線模式，請在網站的呼叫 REDMOND 」 靜態 IPv4 位址，類型：


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


若要建立複製網域控制站名 Clone2 靜態 IPv4 與靜態 IPv6 設定中，輸入離線模式：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


若要建立複製網域控制站離線模式靜態 IPv4 與動態 IPv6 設定中指定 DNS 解析設定多個 DNS 伺服器，鍵入：


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


若要建立複製網域控制站名 Clone1 動態 IPv4 與靜態 IPv6 設定中，輸入離線模式：


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


若要建立複製網域控制站在動態 IPv4 與動態 IPv6 設定離線模式，請輸入：


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>步驟 4： 匯出與然後匯入的來源網域控制站一樣
在這個程序，匯出來源模擬的網域控制站一樣，然後匯入一樣。 這個動作會在您的網域中建立複製模擬的網域控制站。

您需要為每個 HYPER-V 主機上的系統管理員本機群組成員。 如果您的每個伺服器使用不同的認證，執行 Windows PowerShell cmdlet 匯出與匯入 VM，在不同的 Windows PowerShell 工作階段。

如果來源網域控制站快照，它們應該刪除之前來源網域控制站匯出因為 VM 將會匯入快照有與目標超 hyper-v 主機不相容的處理器設定。 如果來源和目標超 hyper-v 主機間的相容的處理器設定，您可能匯出並不事先刪除快照複製來源。 匯入之後，不過，快照必須從刪除複製 VM 開始。

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>若要匯出，然後匯入的來源模擬的網域控制站複製 virtual 網域控制站

1.  在**HyperV1**，關閉來源網域控制站 (**VirtualDC1**)。

    ![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

    停止-VM-命名 VirtualDC1-電腦名稱 HyperV1


2.  在**HyperV1**、 delete 快照，然後匯出 c:\CloneDCs directory 來源網域控制站 (VirtualDC1)。

> [!NOTE]
> 因為取得快照時，每次 AVHD 建立新的檔案會做為差分磁碟，您應該 delete 所有相關聯的快照。 這會建立鏈結影響。 如果您已快照和插入 VHD DCCLoneConfig.xml 檔案，您可能會從舊版 DIT 建立複製或插入錯誤 VHD 檔案中的設定檔。 刪除快照合併所有這些 AVHDs 到 VHD 基底。

![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  複製資料夾**virtualdc1**以 c:\Import directory 的**HyperV2**。

4.  在**HyperV2**、 使用**HYPER-V 管理員**，匯入一樣 (使用**匯入一樣**精靈中**HYPER-V 管理員**) 資料夾**c:\Import\virtualdc1**和 delete 所有相關**快照**。

使用**複製一樣 （建立新的唯一 ID）**選項時匯入一樣。

![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


若要從相同的來源網域控制站建立多個複本網域控制站：

  -   UI： 在 [**匯入一樣**精靈中，指定新的位置**一樣組態資料夾**，**快照網上商店**、**智慧分頁資料夾**，不同**位置**一樣的 virtual 硬碟。

  -   Windows PowerShell： 使用下列的參數指定一樣的新位置`Import-VM`cmdlet:

        $path = Get-childitem 」 C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual 電腦 」 匯入 VM-Path $path.fullname-複製-GenerateNewId-電腦名稱 HyperV2-VhdDestinationPath 「 「-SnapshotFilePath 「 路徑 「-SmartPagingFilePath 「 「-VirtualMachinePath 」 路徑 」


> [!NOTE]
> 建立多個複本網域控制站同時建議批次大小為 10。 最大值限制太多複寫輸出連接的預設為 16 的散發檔案系統複寫 (DFSR) 和 10 檔案複寫服務 (FRS)。 您不應部署以上建議的複製網域控制站同時除非完全已經通過該數字測試您的環境。

5.  在**HyperV1**，重新開機來源網域控制站 (**(VirtualDC1**) 將回上網。

![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  在**HyperV2**，開始一樣 (**VirtualDC2**) 為複製網域控制站網域中將它上網。

![AD DS 簡介](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> 必須執行肯定複製才能繼續。 如果是關機，請確定它已經開始，執行初始同步，也就是知道保留 PDC 模擬器角色。 如需詳細資訊，請查看 Microsoft[知識庫文章 305476](https://support.microsoft.com/kb/305476)。

複製完成之後，請確認已成功複製確保複製到電腦的名稱。 請確認 VM 不開始在 Directory 服務還原模式 (DSRM)。 如果您嘗試登入並收到錯誤，指出不登入伺服器可供使用，請嘗試登入 DSRM。 如果 DC 未成功複製 DSRM 開機，請登入事件檢視器，而帶領登 %systemroot%/debug 資料夾中。

複製的網域控制站會成員的**的網域控制站複製**群組成員資格複製來源網域控制站因為。 做為最佳做法，您應該出發的**的網域控制站複製**群組空白，直到您已經準備好執行複製作業，並複製作業完成之後，您應該會移除成員。

如果來源網域控制站儲存的備份媒體，複製的網域控制站也將會儲存的備份的媒體。 您可以在執行`wbadmin get versions`上的複製的網域控制站顯示備份的媒體。 網域管理群組成員應該 delete 複製的網域控制站防止不小心要還原備份的媒體。 如需如何 delete 使用 wbadmin.exe 系統狀態備份，請查看[Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx)。

## <a name="troubleshooting"></a>疑難排解
如果複製網域控制站 (**VirtualDC2**) 開始在 Directory 服務還原模式 (DSRM)，它不會傳回至標準模式在其上的下一步重新開機。 若要登入的網域控制站在 DSRM 開始使用，請使用**。 \Administrator** ，然後指定 DSRM 密碼。

更正的項目複製失敗的原因，並確認 dcpromo.log 不會指出複製無法嘗試重新。 複製無法嘗試重新，安全捨棄媒體。 如果複製重新嘗試，您必須以再試一次複製移除 DS 還原模式開機旗標。

1.  開放的 Windows Server 2012，以提升權限的命令 （向按 Windows Server 2012，選擇 [以系統管理員身分執行），然後輸入**msconfig**。

2.  在**開機**索引標籤，在**開機選項**，清除**安全開機**(它已選取的選項**功能的 Active Directory 修復**)。

3.  按一下**[確定]**並重新出現提示時。

如需有關模擬的網域控制站疑難排解資訊，請查看[擬化檔案網域控制站疑難排解](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md)。


