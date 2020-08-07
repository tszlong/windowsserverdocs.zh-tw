---
ms.assetid: 07d6b251-c492-4d9f-bcc4-031023695b24
title: 安裝並啟用重複資料刪除
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
description: 在 Windows Server 上安裝「重複資料刪除」的方式，取決於工作負載是否是不錯的「重複資料刪除」候選，並且在磁碟區上啟用「重複資料刪除」。
ms.openlocfilehash: 44a94f5bfd3a715a1869d79f25b62833ed8b5eb3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936407"
---
# <a name="install-and-enable-data-deduplication"></a>安裝並啟用重複資料刪除
> 適用於 Windows Server (半年度管道)、Windows Server 2016

本主題說明如何安裝[重複資料刪除](overview.md)、評估重複資料刪除的工作負載，以及在特定磁碟區上啟用重複資料刪除。

> [!Note]
> 如果您打算在容錯移轉叢集中執行「重複資料刪除」，該叢集的每個節點都必須安裝「重複資料刪除」伺服器角色。

## <a name="install-data-deduplication"></a><a id="install-dedup"></a>安裝重複資料刪除
> [!Important]
> [KB4025334](https://support.microsoft.com/kb/4025334) 包含重複資料刪除的修正彙總套件，包括重要的可靠性修正，我們極力建議您在 Windows Server 2016 上使用重複資料刪除時安裝它。

### <a name="install-data-deduplication-by-using-server-manager"></a><a id="install-dedup-via-server-manager"></a>使用伺服器管理員安裝重複資料刪除
1. 在 [新增角色及功能精靈] 中，選取 [伺服器角色]****，然後選取 [重複資料刪除]****。
![透過伺服器管理員安裝重複資料刪除︰從 [伺服器角色] 中選取 [重複資料刪除]](media/install-dedup-via-server-manager-1.png)
2. 按一下 [下一步]**** 直到 [安裝]**** 按鈕被啟用，然後按一下 [安裝]****。
![透過伺服器管理員安裝重複資料刪除：按一下 [安裝]](media/install-dedup-via-server-manager-2.png)

### <a name="install-data-deduplication-by-using-powershell"></a><a id="install-dedup-via-powershell"></a>使用 PowerShell 安裝重複資料刪除
若要安裝重復資料刪除，請以系統管理員身分執行下列 PowerShell 命令：`Install-WindowsFeature -Name FS-Data-Deduplication`

若要在 Nano 伺服器安裝中安裝重複資料刪除︰

1. 利用[開始使用 Nano 伺服器](../../get-started/getting-started-with-nano-server.md)所述的已安裝存放裝置，建立 Nano 伺服器安裝。
2. 從執行 Nano 伺服器以外之任何模式的 Windows Server 2016 伺服器中，或從已安裝[遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520) (RSAT) 的 Windows 電腦中，將具有明確參考的重複資料刪除安裝到 Nano 伺服器執行個體上 (以 Nano 伺服器執行個體的實際名稱取代 'MyNanoServer')：
    ```PowerShell
    Install-WindowsFeature -ComputerName <MyNanoServer> -Name FS-Data-Deduplication
    ```
    <br />
    <strong>--或--</strong>
    <br />
    使用 PowerShell 遠端執行功能遠端連線至 Nano 伺服器執行個體，然後使用 DISM 安裝重複資料刪除：

    ```PowerShell
    Enter-PSSession -ComputerName MyNanoServer
    dism /online /enable-feature /featurename:dedup-core /all
    ```

## <a name="enable-data-deduplication"></a><a id="enable-dedup"></a>啟用重複資料刪除
### <a name="determine-which-workloads-are-candidates-for-data-deduplication"></a><a id="enable-dedup-candidate-workloads"></a>決定可進行重複資料刪除的候選工作負載
重複資料刪除可透過減少重複資料所耗用的磁碟空間量，有效地將伺服器應用程式的資料消耗量成本降至最低。 啟用重複資料刪除之前，請務必了解您工作負載的特性，以確保存放裝置能夠發揮最大效能。 有兩種工作負載類別需要考量：

* **「建議的工作負載」，此類別已證明同時具有能高度受益於重複資料刪除的兩個資料集，且具有與重複資料刪除之後續處理模型相容的資源耗用量模式。 建議您一律在下列的工作負載上[啟用重複資料刪除](install-enable.md#enable-dedup-lights-on)：
    * 提供共用的一般用途檔案伺服器 (GPFS)，例如小組共用、使用者主資料夾、工作資料夾，以及軟體開發共用。
    * 虛擬桌面基礎結構 (VDI) 伺服器。
    * 虛擬化備份應用程式，例如[Microsoft Data Protection Manager (DPM) ](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12))。
* 可能受益於重複資料刪除，但並非總是重複資料刪除良好候選的工作負載。 例如，下列工作負載可能適合進行重複資料刪除，但您應該先評估重複資料刪除的優點︰
    * 一般用途的 Hyper-V 主機
    * SQL Server
    * 企業營運 (LOB) 伺服器

### <a name="evaluate-workloads-for-data-deduplication"></a><a id="enable-dedup-evaluating-sometimes-workloads"></a>評估重複資料刪除的工作負載
> [!Important]
> 如果您是執行建議的工作負載，則可以略過本節，並直接為工作負載[啟用重複資料刪除](install-enable.md#enable-dedup-lights-on)。

若要判斷工作負載是否適合進行重複資料刪除，請回答下列問題。 如果您對工作負載感到不確定，請考慮為工作負載在測試資料集上執行重複資料刪除的試驗部署，以查看它的執行情況。

1. **我的工作負載資料集是否具有足夠的重複資料量，以受益於啟用重複資料刪除？**
    在為工作負載啟用重複資料刪除之前，請使用重複資料刪除節省評估工具 (或稱為 DDPEval) 調查您工作負載資料集的重複資料量。 安裝重複資料刪除之後，您可以在下列位置找到此工具：`C:\Windows\System32\DDPEval.exe`。 DDPEval 可針對直接連線的磁碟區 (包括本機磁碟機或叢集共用磁碟區)，以及對應或未對應的網路共用，評估最佳化的可能性。
    &nbsp;執行 DDPEval.exe 將會傳回類似下列的輸出： &nbsp;`Data Deduplication Savings Evaluation Tool`
    `Copyright 2011-2012 Microsoft Corporation.  All Rights Reserved.`
    &nbsp; `Evaluated folder: E:\Test`
    `Processed files: 34`
    `Processed files size: 12.03MB`
    `Optimized files size: 4.02MB`
    `Space savings: 8.01MB`
    `Space savings percent: 66`
    `Optimized files size (no compression): 11.47MB`
    `Space savings (no compression): 571.53KB`
    `Space savings percent (no compression): 4`
    `Files with duplication: 2`
    `Files excluded by policy: 20`
    `Files excluded by error: 0`

2. **我的工作負載對其資料集的 i/o 模式看起來像什麼？我的工作負載有何效能？**
     重複資料刪除會將檔案最佳化為定期工作，而不是將檔案寫入至磁碟時。 因此，一定要檢查的是工作負載對已經過重複資料刪除處理之磁碟區的預期讀取模式。 由於重複資料刪除會將檔案內容移入區塊存放區中，並嘗試盡可能依檔案來組織區塊存放區，因此針對檔案循序範圍所套用的讀取作業，將會有最佳的效能。

    類資料庫的工作負載通常具有較為隨機的讀取模式 (而非循序讀取模式)，因為資料庫通常並不會保證資料庫配置會針對所有可能執行的查詢進行最佳化。 由於區塊存放區的區段可能會分散在磁碟區各處，因此針對資料庫查詢存取區塊存放區中的資料範圍，可能會產生額外的延遲。 高效能的工作負載特別容易受到上述額外延遲的影響，但其他類資料庫的工作負載可能不會。

    > [!Note]
    > 這些考量主要適用於由傳統旋轉式儲存媒體 (也稱為硬碟磁碟機或 HDD) 所組成之磁碟區上的存放裝置工作負載。 全快閃存放裝置基礎結構 (也稱為固態硬碟磁碟機或 SSD) 較不會受到隨機 IO 模式的影響，原因在於快閃媒體的特性之一便是針對媒體上所有位置都具有相同的存取時間。 因此，重複資料刪除針對儲存在全快閃媒體上之工作負載資料集所產生的讀取延遲，與在傳統旋轉式儲存媒體上將會不同。

3. **我在伺服器上的工作負載會有哪些資源需求？**
    由於重複資料刪除是使用後續處理模型，因此重複資料刪除將定期需要有足夠的系統資源以完成[最佳化和其他工作](understand.md#job-info)。 這表示具有閒置時間 (例如晚上或週末) 的工作負載最適合進行重複資料刪除，而需要全天候執行的工作負載則較不適合。 沒有任何閒置時間的工作負載如果在伺服器上的資源需求不高，則該工作負載仍然可能適合進行重複資料刪除。

### <a name="enable-data-deduplication"></a><a id="enable-dedup-lights-on"></a>啟用重複資料刪除
啟用重複資料刪除功能之前，您必須選擇與您的工作負載最類似的[使用類型](understand.md#usage-type)。 重複資料刪除包含的使用類型有三種。

* [預設](understand.md#usage-type-default)：專為一般用途的檔案伺服器調整
* [HYPER-V](understand.md#usage-type-hyperv)：專為 VDI 伺服器調整
* [備份](understand.md#usage-type-backup)：專為虛擬備份應用程式調整，例如 [Microsoft DPM](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12))

#### <a name="enable-data-deduplication-by-using-server-manager"></a><a id="enable-dedup-via-server-manager"></a>使用伺服器管理員啟用重複資料刪除
1. 在伺服器管理員中選取 [檔案**和存放服務**]。
![按一下 [檔案和存放服務]](media/enable-dedup-via-server-manager-1.PNG)
2. 從 [檔案和存放服務]**** 中，選取 [磁碟區]****。
![按一下磁碟區](media/enable-dedup-via-server-manager-2.png)
3. 在所需的磁碟區上按一下滑鼠右鍵，然後選取 [設定重複資料刪除]****。
![按一下 [設定重複資料刪除]](media/enable-dedup-via-server-manager-3.png)
4. 從下拉式清單方塊中選取所需的 [使用類型]****，然後選取 [確定]****。
![從下拉式清單中選取所需的 [使用類型]](media/enable-dedup-via-server-manager-4.png)
5. 如果您是執行建議的工作負載，即大功告成。 針對其他工作負載，請參閱[其他考量](#enable-dedup-sometimes-considerations)。

> [!Note]
> 您可以在 [[設定重複資料刪除](advanced-settings.md)] 頁面上找到排除副檔名或資料夾，以及選取重複資料刪除排程的詳細資訊 (包括這麼做的原因)。

#### <a name="enable-data-deduplication-by-using-powershell"></a><a id="enable-dedup-via-powershell"></a>使用 PowerShell 啟用重複資料刪除
1. 使用系統管理員內容，執行下列 PowerShell 命令︰
    ```PowerShell
    Enable-DedupVolume -Volume <Volume-Path> -UsageType <Selected-Usage-Type>
    ```

2. 如果您是執行建議的工作負載，即大功告成。 針對其他工作負載，請參閱[其他考量](#enable-dedup-sometimes-considerations)。

> [!Note]
> 重復資料刪除 PowerShell Cmdlet （包括 [`Enable-DedupVolume`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) ）可以從遠端執行，方法是在 `-CimSession` CIM 會話附加參數。 這特別適用於從遠端針對 Nano 伺服器執行個體執行重複資料刪除 PowerShell Cmdlet。 若要建立新的 CIM 會話，請執行 [`New-CimSession`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) 。

#### <a name="other-considerations"></a><a id="enable-dedup-sometimes-considerations"></a>其他考量
> [!Important]
> 如果您是執行建議的工作負載，則可以略過本節。

* 重複資料刪除的使用類型會針對建議的工作負載提供合理的預設值，但也能夠為所有的工作負載提供不錯的起點。 針對建議的工作負載以外的工作負載，您可以修改[重複資料刪除的進階設定](advanced-settings.md)，來提升重複資料刪除效能。
* 如果您的工作負載在伺服器上具有較高的資源需求，重複資料刪除工作[應該安排在該工作負載預期的閒置期間內執行](advanced-settings.md#modifying-job-schedules-change-schedule)。 這在超交集主機上執行重複資料刪除時特別重要，因為在預期的工作期間內執行重複資料刪除將會佔用 VM。
* 如果您工作負載的資源需求不高，或者完成最佳化工作比起完成工作負載要求更為重要，則您可以[調整記憶體、CPU，以及重複資料刪除工作的優先順序](advanced-settings.md#modifying-job-schedules)。

## <a name="frequently-asked-questions-faq"></a><a id="faq"></a>常見問題 (常見問題) 
**我想要在 X 工作負載的資料集上執行重復資料刪除。這是支援的嗎？**
除了[已知無法與重複資料刪除功能相互操作](interop.md)的工作負載之外，我們完全支援搭配任何工作負載之重複資料刪除的資料完整性。 建議的工作負載也受到 Microsoft 針對效能上的支援。 其他工作負載的效能大量取決於它們對您伺服器上執行的作業。 您必須判斷重複資料刪除對您工作負載造成的效能影響，以及該影響對此工作負載是否可以接受。

**重複資料刪除磁碟區的磁碟區大小需求為何？**
在 Windows Server 2012 與 Windows Server 2012 R2 中，使用者必須仔細調整磁碟區大小，以確保重複資料刪除可以跟上磁碟區上資料量變換的步調。 這通常表示工作負載變換度較高之已經過重複資料刪除處理的磁碟區平均大小上限為 1 至 2 TB，而建議的絕對大小上限為 10 TB。 在 Windows Server 2016 中，這些限制已經被移除。 如需詳細資訊，請參閱[重複資料刪除的新功能](whats-new.md#large-volume-support)。

**我需要為建議的工作負載修改排程或其他重複資料刪除設定嗎？**
否，我們所提供的[使用類型](understand.md#usage-type)能夠為建議的工作負載提供合理的預設值。

**重複資料刪除的記憶體需求為何？**
重複資料刪除最少應該要有 300 MB 的基本記憶體，並針對每 1 TB 的邏輯資料額外增加 50 MB 的記憶體。 比方說，如果您要最佳化 10 TB 的磁碟區，您最少需要配置 800 MB 的記憶體，以供進行重複資料刪除 (`300 MB + 50 MB * 10 = 300 MB + 500 MB = 800 MB`)。 雖然重複資料刪除可以利用最低限度的記憶體容量對磁碟區進行最佳化，如此有限的資源將會減緩重複資料刪除工作的速度。

最佳情況是，重複資料刪除針對每 1 TB 的邏輯資料，應該要有 1 GB 的記憶體。 比方說，如果您要最佳化 10 TB 的磁碟區，您最好配置 10 GB 的記憶體，以供進行重複資料刪除 (`1 GB * 10`)。 這個比率將能確保重複資料刪除工作具有最高效能。

**重複資料刪除的儲存體需求為何？**
在 Windows Server 2016 中，重複資料刪除可支援最多 64 TB 的磁碟區大小。 如需詳細資訊，請檢視[重複資料刪除的新功能](whats-new.md#large-volume-support)。
