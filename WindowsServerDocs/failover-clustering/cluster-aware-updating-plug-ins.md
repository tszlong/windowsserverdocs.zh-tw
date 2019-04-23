---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: 叢集感知更新外掛程式的運作方式
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: 如何使用外掛程式來協調更新時使用叢集感知更新 Windows Server 中的叢集上安裝更新。
ms.openlocfilehash: d09addb5e6787a8386d50570c0d27640646aa587
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854559"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>叢集感知更新外掛程式的運作方式

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

[叢集感知更新](cluster-aware-updating.md)(CAU) 使用外掛程式來協調容錯移轉叢集中的節點之間的更新的安裝。 本主題提供使用內建的相關資訊\-在 CAU 外掛程式\-集] 或 [其他隨插即用\-您為 CAU 安裝的單元。

## <a name="BKMK_INSTALL"></a>安裝隨插即用\-中  
隨插即用\-在非預設值插入\-隨 CAU 安裝的 ins \( **Microsoft.WindowsUpdatePlugin**並**Microsoft.HotfixPlugin** \)必須另外安裝。 如果使用 CAU 中自我\-更新模式中，隨插即用\-中必須安裝在所有叢集節點上。 如果在遠端使用 CAU\-更新模式中，隨插即用\-中必須安裝在遠端更新協調器電腦上。 隨插即用\-，在您安裝可能會每個節點上有額外的安裝需求。  
  
若要安裝外掛程式\-中，請遵循指示從隨插即用\-在 「 發行者 」。 手動註冊外掛程式\-使用 CAU，執行[Register-cauplugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet 的每部電腦上其中隨插即用\-安裝中。  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>指定隨\-中，插\-in 引數  
  
### <a name="specify-a-cau-plug-in"></a>指定 CAU 外掛程式\-中

在 CAU UI 中，選取 隨\-中從卸除\-可用的外掛程式清單中向\-集，當您使用 CAU 來執行下列動作：  
  
-   套用更新至叢集  
  
-   預覽叢集的更新  
  
-   設定叢集自行\-更新選項  
  
根據預設，CAU 會選取外掛程式\-中**Microsoft.WindowsUpdatePlugin**。 不過，您可以在其中指定任何隨插即用\-，因為已安裝並註冊 CAU 使用。

> [!TIP]  
> 在 CAU UI 中，您可以只指定單一的隨插即用\-中供 CAU 使用來預覽或套用更新執行 」 期間的更新。 使用 CAU 的 PowerShell cmdlet，您可以指定一或多個隨插即用\-集。如果您需要在叢集上安裝多個類型的更新，會指定多個外掛程式通常更有效率\-一個 「 更新執行，而不是每個外掛程式使用個別更新執行中的單元\-中。 例如，通常需要重新啟動的節點數目較少。

藉由使用下列表格中所列的 CAU PowerShell cmdlet，您可以指定一或多個外掛程式\-單元的 「 更新執行 」 或藉由傳遞的掃描 **– CauPluginName**參數。 您可以指定多個隨插即用\-中使用逗號分隔的名稱。 如果您指定多個外掛程式\-集，您也可以控制如何隨\-影響彼此更新執行 」 期間藉由指定 **\-RunPluginsSerially**，  **\-StopOnPluginFailure**，並 **– SeparateReboots**參數。 如需有關使用多個隨插即用\-下表中的 cmdlet 文件集，使用提供的連結。  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|新增 CAU 叢集的角色，提供自助\-更新的功能，以指定的叢集。|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|執行叢集節點掃描以判斷適用的更新，並透過所指定叢集上的「更新執行」安裝那些更新。|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|執行叢集節點掃描以判斷適用的更新，並傳回將套用至所指定叢集中每個節點的初始更新集清單。|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|設定所指定叢集上的 CAU 叢集角色屬性。|  
  
如果您未指定 CAU 外掛程式\-使用這些 cmdlet 的參數，預設值是外掛程式\-中**Microsoft.WindowsUpdatePlugin**。  
  
### <a name="specify-cau-plug-in-arguments"></a>指定 CAU 插入\-in 引數  
當您設定 「 更新執行 」 選項時，您可以指定一或多個*名稱\=值*配對\(引數\)針對所選的隨插即用\-才可使用。 例如，在 CAU UI 中，您可以指定多個引數，如下所示：  
  
**Name1\=Value1;Name2\=Value2;Name3\=Value3**  
  
這些*名稱\=值*組必須是有意義的隨插即用\-，因為您所指定。 針對某些隨插即用\-集引數是選擇性的。  
  
CAU 外掛程式的語法\-in 引數會遵循這些一般規則：  
  
-   多個*名稱\=值*會以分號分隔組。  
  
-   包含空格的值必須以引號括住，例如：**Name1\=「 有空格的值 」**。  
  
-   確切語法*值*取決於隨插即用\-中。  
  
若要指定隨\-引數中使用支援 CAU PowerShell cmdlet **– CauPluginParameters**參數，傳遞格式的參數：  
  
**\-CauPluginArguments @{Name1\=Value1;Name2\=Value2;Name3\=Value3}**  
  
您也可以使用預先定義的 PowerShell 雜湊表。 若要指定隨插即用\-引數中的多個隨插即用\-中，將傳遞的引數，以逗號分隔的多個雜湊資料表。 傳遞隨\-中的隨插即用中的引數\-的順序中指定**CauPluginName**。  
  
### <a name="specify-optional-plug-in-arguments"></a>指定選擇性的隨插即用\-in 引數  
隨插即用\-CAU 安裝的 ins \( **Microsoft.WindowsUpdatePlugin**並**Microsoft.HotfixPlugin** \)提供您可以選取的其他選項。 在 CAU UI 中，這些會出現在**其他選項**頁面上設定的隨插即用的 「 更新執行 」 選項之後\-中。 如果您使用 CAU 的 PowerShell cmdlet，這些選項會設定為選擇性的隨插即用\-in 引數。 如需詳細資訊，請參閱本主題稍後的 [使用 Microsoft.WindowsUpdatePlugin](#BKMK_WUP) 和 [使用 Microsoft.HotfixPlugin](#BKMK_HFP) 。  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>管理外掛程式\-集使用 Windows PowerShell cmdlet  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|擷取一或多個軟體更新外掛程式的相關資訊\-，可用來在本機電腦上註冊。|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|登錄 CAU 軟體更新外掛程式\-在本機電腦上。|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|移除軟體更新外掛程式\-在清單中的隨插即用\-可供 CAU 的單元。 **注意：** 隨插即用\-隨 CAU 安裝的 ins \( **Microsoft.WindowsUpdatePlugin**並**Microsoft.HotfixPlugin** \)無法取消註冊。|  
  
## <a name="BKMK_WUP"></a>使用 Microsoft.WindowsUpdatePlugin  

預設值隨\-中的 CAU， **Microsoft.WindowsUpdatePlugin**，執行下列動作：
- 與每個容錯移轉叢集節點上的「Windows Update 代理程式」通訊，以套用每個節點上執行之 Microsoft 產品所需的更新。
- 安裝叢集的直接更新，從 Windows Update 或 Microsoft Update，或在\-內部部署 Windows Server Update Services \(WSUS\)伺服器。
- 只安裝選取、 一般發行版本\(GDR\)更新。 根據預設，隨插即用\-在套用 只有重要的軟體更新。 不需要進行任何設定。 預設設定會在每個節點上下載並安裝重要 GDR 更新。 

> [!NOTE]
> 若要套用預設會選取的重要軟體更新以外的更新\(驅動程式的更新，例如\)，您可以設定選用的隨插即用\-參數中。 如需詳細資訊，請參閱[設定 Windows Update 代理程式查詢字串](#BKMK_QUERY)。

### <a name="requirements"></a>需求

- 容錯移轉叢集與遠端更新協調器電腦\(如果使用\)必須符合 CAU 和所需中所列的遠端管理設定需求[需求和針對 CAU 最佳做法](cluster-aware-updating-requirements.md).
- 檢閱[套用 Microsoft 更新的建議](cluster-aware-updating-requirements.md#BKMK_BP_WUA)，接著針對容錯移轉叢集節點進行所需的任何 Microsoft Update 設定變更。
- 為了獲得最佳結果，我們建議您執行 「 CAU 最佳做法分析程式\(BPA\)確保叢集與更新環境已正確設定，以使用 CAU 來套用更新。 如需詳細資訊，請參閱[測試 CAU 更新整備](cluster-aware-updating-requirements.md#BKMK_BPA)。

> [!NOTE]
> 系統會排除需要接受 Microsoft 授權條款或需要使用者互動的更新，您需要手動安裝這些更新。

### <a name="additional-options"></a>其他選項

（選擇性） 您可以指定下列外掛程式\-引數，來擴充或限制的隨插即用會套用的更新集合中\-中：
- 若要設定外掛程式\-上套用建議的更新，除了重要更新每個節點，在 CAU UI 中，在**其他選項**頁面上，選取**讓我建議您更新相同採用的方式接收重要更新**核取方塊。
<br>或者，設定 **'IncludeRecommendedUpdates'\='True'** 插入\-引數中。
- 若要設定外掛程式\-在若要篩選的會套用至每個叢集節點的 GDR 更新類型，指定 Windows Update Agent 查詢字串使用**QueryString**插入\-引數中。 如需詳細資訊，請參閱[設定 Windows Update 代理程式查詢字串](#BKMK_QUERY)。

### <a name="BKMK_QUERY"></a>設定 Windows Update Agent 查詢字串  
您可以設定外掛程式\-預設值的引數中插入\-在中， **Microsoft.WindowsUpdatePlugin**，其中包含 Windows Update 代理程式\(WUA\)查詢字串。 此指示使用 WUA API 來識別要套用到每個節點的一或多個 Microsoft 更新群組 (根據特定選取範圍條件)。 您可以使用邏輯 AND 或邏輯 OR 來結合多個條件。 WUA 查詢字串中的隨插即用指定\-in 引數，如下所示：  
  
**QueryString\="Criterion1\=Value1 並\/或 Criterion2\=Value2 和\/或...」**  
  
例如，**Microsoft.WindowsUpdatePlugin** 會使用預設的 **QueryString** 引數 (使用 **IsInstalled**、**Type**、**IsHidden** 與 **IsAssigned** 條件所建構) 自動選取重要更新。  
  
**QueryString\="IsInstalled\=0 和型別\=「 軟體 」 和 IsHidden\=0 和 IsAssigned\=1 」**  
  
如果您指定**QueryString**引數，它用來取代預設**QueryString**針對外掛程式設定\-中。  
  
#### <a name="example-1"></a>範例 1
  
若要設定**QueryString**安裝特定更新識別碼所識別的引數*f6ce46c1\-971 c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\="UpdateID\=' f6ce46c1\-971 c\-43f9\-a2aa\-783df125f003' 和 IsInstalled\=0 」**  
  
> [!NOTE]  
> 上述範例中是使用叢集來套用更新的有效\-注意更新精靈。 如果您想要安裝特定更新，藉由設定自我\-更新選項搭配 CAU UI 或使用**新增\-CauClusterRole**或是**設定\-CauClusterRole**PowerShell cmdlet，您必須使用兩個單一 UpdateID 值的格式\-加上引號的字元：  
>   
> **QueryString\="UpdateID\=''f6ce46c1\-971c\-43f9\-a2aa\-783df125f003'' and IsInstalled\=0"**  
  
#### <a name="example-2"></a>範例 2
  
設定只會安裝驅動程式的 **QueryString** 引數：  
  
**QueryString\="IsInstalled\=0 和型別\=「 Driver 」 和 IsHidden\=0 」**  
  
如需有關查詢字串的預設值插入\-中， **Microsoft.WindowsUpdatePlugin**，搜尋準則\(這類**IsInstalled**\)，您可以在查詢字串，包含語法看一節中有關搜尋條件[Windows Update Agent (WUA) API 參照](https://go.microsoft.com/fwlink/p/?LinkId=223304)。  
  
## <a name="BKMK_HFP"></a>使用 Microsoft.HotfixPlugin  
插頭\-中**Microsoft.HotfixPlugin**可用來套用 Microsoft 限定發行版本\(release，LDR\)更新\(也稱為 hotfix，先前稱為 Qfe\) ，則您另行下載來解決特定 Microsoft 軟體問題。 外掛程式的根資料夾中的 SMB 檔案共用上安裝更新和也可以自訂以套用非\-Microsoft 驅動程式、 韌體與 BIOS 更新。

> [!NOTE]
> Hotfix 有時候是可供下載 microsoft 知識庫文件，但它們也會提供給客戶，視\-所需的基礎。

### <a name="requirements"></a>需求

- 容錯移轉叢集與遠端更新協調器電腦\(如果使用\)必須符合 CAU 和所需中所列的遠端管理設定需求[需求和針對 CAU 最佳做法](cluster-aware-updating-requirements.md).
- 檢閱[使用 Microsoft.HotfixPlugin 的建議](cluster-aware-updating-requirements.md#BKMK_BP_HF)。
- 為了獲得最佳結果，我們建議您執行 「 CAU 最佳做法分析程式\(BPA\)模型，以確保叢集與更新環境已正確設定，以使用 CAU 來套用更新。 如需詳細資訊，請參閱[測試 CAU 更新整備](cluster-aware-updating-requirements.md#BKMK_BPA)。
- 從發行者取得更新並將其複製或解壓縮至伺服器訊息區塊\(SMB\)檔案共用\(hotfix 根資料夾\)至少支援 SMB 2.0，這是由叢集中的所有存取節點與遠端更新協調器 」 電腦\(如果在遠端使用 CAU\-更新模式\)。 如需詳細資訊，請參閱此主題稍後的[設定 Hotfix 根資料夾結構](#BKMK_HF_ROOT)。 

    > [!NOTE]
    > 根據預設，此外掛程式\-中只會安裝 hotfix 具有下列副檔名：.msu、.msi 與.msp。

- 將 DefaultHotfixConfig.xml 檔案複製\(這篇 **%systemroot%\\System32\\WindowsPowerShell\\v1.0\\模組\\ClusterAwareUpdating**資料夾的電腦上安裝 CAU 工具\)到您所建立的 hotfix 根資料夾並將 hotfix 解壓縮。 例如，組態將檔案複製到 *\\ \\MyFileServer\\Hotfix\\根\\*。 

    > [!NOTE]
    > 若要安裝 Microsoft 提供的大部分 Hotfix 與其他更新，您可以使用預設的 Hotfix 設定檔而無需修改。 若您的案例需要，您可以以進階工作方式自訂設定檔。 設定檔可以包含自訂規則，例如處理具有特定副檔名的 Hotfix 或定義特定結束條件行為的規則。 如需詳細資訊，請參閱此主題稍後的[自訂 Hotfix 設定檔](#BKMK_CONFIG_FILE)。

### <a name="configuration"></a>組態

設定下列設定。 如需詳細資訊，請參閱此主題稍後的小節的連結。
- 包含要套用之更新與 Hotfix 設定檔之共用 Hotfix 根資料夾的路徑。 您可以在 CAU UI 中輸入此路徑，或設定**HotfixRootFolderPath\=\<路徑 >** PowerShell 隨插即用\-引數中。 

   > [!NOTE]
   > 您可以在作為本機資料夾路徑或 UNC 路徑的形式指定 hotfix 根資料夾 *\\ \\ServerName\\共用\\RootFolderName*。 網域\-或獨立 DFS 命名空間路徑可用。 不過，隨插即用\-檢查存取權的功能在 hotfix 設定檔中的權限與不相容的 DFS 命名空間路徑，因此若您已設定，您必須停用存取權限的核取使用 CAU UI 或透過設定**DisableAclChecks\='True'** 插入\-引數中。
- 在裝載 hotfix 根資料夾的適當權限存取的資料夾，並確定從 SMB 存取資料的完整性檢查的伺服器上設定共用資料夾\(SMB 簽署或 SMB 加密\)。 如需詳細資訊，請參閱 [限制對 Hotfix 根資料夾的存取](#BKMK_ACL)。

### <a name="additional-options"></a>其他選項

- （選擇性） 設定外掛程式\-在因此 SMB 加密會強制執行從 hotfix 檔案共用中存取資料時。 在 CAU UI 中，在**其他選項**頁面上，選取**中的 存取 hotfix 根資料夾時需要 SMB 加密**選項，或設定**RequireSMBEncryption\='True'** PowerShell 隨插即用\-引數中。 
  > [!IMPORTANT]
  > 您必須在 SMB 伺服器上執行額外的設定步驟，以使用 SMB 簽署或 SMB 加密來啟用 SMB 資料完整性。 如需詳細資訊，請參閱[限制對 Hotfix 根資料夾的存取](#BKMK_ACL)中的步驟 4。 若選取強制使用 SMB 加密的選項，且未使用 SMB 加密來設定對 Hotfix 根資料夾的存取權，「更新執行」將失敗。
- 或者，您可以停用是否有足夠權限可存取 Hotfix 根資料夾與 Hotfix 設定檔的預設檢查。 在 CAU UI 中，選取**停用 以系統管理員存取 hotfix 根資料夾和設定檔的核取**，或設定**DisableAclChecks\='True'** 插入\-中引數。
- （選擇性） 設定**HotfixInstallerTimeoutMinutes\= <Integer>** 引數來指定時間的 hotfix 插入\-中等候 hotfix 安裝程式處理序，來傳回。 \(預設值為 30 分鐘。\)例如，若要指定兩個小時逾時期間內，設定**HotfixInstallerTimeoutMinutes\=120**。
- （選擇性） 設定**HotfixConfigFileName \= <name>** 插入\-中引數來指定位於 hotfix 根資料夾的 hotfix 設定檔的名稱。 若未指定，會使用預設名稱 DefaultHotfixConfig.xml。
  
### <a name="BKMK_HF_ROOT"></a>設定 hotfix 根資料夾結構

為了讓 hotfix 插入\-中運作，hotfix 必須儲存在良好\-SMB 檔案共用中所定義結構\(hotfix 根資料夾\)，而且您必須設定 hotfix 外掛程式\-使用的路徑使用 CAU UI 或 CAU PowerShell cmdlet 的 hotfix 根資料夾。 此路徑會傳遞至插頭\-中為**HotfixRootFolderPath**引數。 您可以根據您的更新需求為 Hotfix 根資料夾選擇數種結構的其中之一，如下列範例所示。 未遵守該結構的檔案或資料夾會被忽略。  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>範例 1-資料夾結構用來將 hotfix 套用到所有叢集節點
  
若要指定 hotfix 套用到所有叢集節點，將它們複製到名為的資料夾**CAUHotfix\_所有**hotfix 根資料夾下。 在此範例中， **HotfixRootFolderPath**插入\-引數中設 *\\ \\MyFileServer\\Hotfix\\根\\*. **CAUHotfix\_所有**資料夾包含三個具有.msu、.msi 與.msp 副檔名，將會套用到所有叢集節點的更新。 更新檔案名稱只適用於示範用途。  
  
> [!NOTE]  
> 在此範例與下列範例中，具有此預設名稱 (DefaultHotfixConfig.xml) 的 Hotfix 設定檔是顯示在 Hotfix 根資料夾的必要位置。  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>為特定的更新只套用至特定節點所使用的範例 2-資料夾結構
  
若要指定只將 Hotfix 套用到特定節點，請使用 Hotfix 根資料夾下具有節點名稱的子資料夾。 使用叢集節點的 NetBIOS 名稱，例如 *ContosoNode1*。 接著，將只要套用到此節點的更新移動到此子資料夾中。 在下列範例中， **HotfixRootFolderPath**插入\-引數中設 *\\ \\MyFileServer\\Hotfix\\根\\*. 中的更新**CAUHotfix\_所有**資料夾將會套用至所有叢集節點，以及*Node1\_特定\_Update.msu*將只會套用到*ContosoNode1*。  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
   ContosoNode1\   
      Node1_Specific_Update.msu   
      ...  
```  
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>範例 3-資料夾結構用於套用.msu、.msi 與.msp 檔案以外的更新
  
根據預設值，**Microsoft.HotfixPlugin** 只會套用具有 .msu、.msi 或 .msp 副檔名的更新。 不過，特定更新可能會有不同的副檔名，而且需要不同的安裝命令。 例如，您可能需要將具有副檔名 .exe 的韌體更新套用到叢集中的節點。 您可以設定 hotfix 根資料夾子資料夾，以指出特定的非\-應該安裝預設更新類型。 您也必須設定對應的資料夾安裝規則，以在 Hotfix 設定 XML 檔案的 `<FolderRules>` 元素中指定安裝命令。  
  
在下列範例中， **HotfixRootFolderPath**插入\-引數中設 *\\ \\MyFileServer\\Hotfix\\根\\*. 有數個更新將套用到所有叢集節點，有一個韌體更新 *SpecialHotfix1.exe* 將套用到 *ContosoNode1* (使用 *FolderRule1*)。 如需有關在 Hotfix 設定檔中設定 *FolderRule1* 的資訊，請參閱此主題稍後的 [自訂 Hotfix 設定檔](#BKMK_CONFIG_FILE) 。  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
  
   ContosoNode1\   
      FolderRule1\  
          SpecialHotfix1.exe  
      ...  
```

### <a name="BKMK_CONFIG_FILE"></a>自訂 hotfix 設定檔  
Hotfix 設定檔控制 **Microsoft.HotfixPlugin** 如何在容錯移轉叢集中安裝特定的 Hotfix 檔案類型。 設定檔的 XML 結構描述是在 HotfixConfigSchema.xsd 中定義，此 .xsd 檔案位於已安裝 CAU 工具之電腦的下列資料夾：  
  
**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating folder**  
  
若要自訂 Hotfix 設定檔，請將範例設定檔 DefaultHotfixConfig.xml 從此位置複製到 Hotfix 根資料夾，並根據您的案例進行適當的修改。  
  
> [!IMPORTANT]  
> 若要套用 Microsoft 提供的大部分 Hotfix 與其他更新，您可以使用預設的 Hotfix 設定檔而無需修改。 Hotfix 設定檔自訂是進階使用案例中的工作。  
  
根據預設值，Hotfix 設定 XML 檔案會定義下列兩種 Hotfix 類別的安裝規則與結束條件：  
  
-   具有副檔名的 Hotfix 檔案的插頭\-在可以安裝預設\(.msu、.msi 與.msp 檔案\)。  
  
    這些是定義為 `<ExtensionRules>` 元素中的 `<DefaultRules>` 元素。 每個預設支援的檔案類型都有一個 `<Extension>` 元素。 一般 XML 結構如下所示：  
  
    ```xml  
    <DefaultRules>  
        <ExtensionRules>  
          <Extension name="MSI">  
            <!-- Template and ExitConditions elements for installation of .msi files follow -->  
             ...  
          </Extension>  
          <Extension name="MSU">  
            <!-- Template and ExitConditions elements for installation of .msu files follow -->  
             ...  
          </Extension>  
          <Extension name="MSP">  
            <!-- Template and ExitConditions elements for installation of .msp files follow -->  
             ...  
          </Extension>  
             ...  
       </ExtensionRules>  
    </DefaultRules>  
    ```  
  
    若需要將特定更新類型套用到您環境中的所有叢集節點，您可以定義額外的 `<Extension>` 元素。  
  
-   Hotfix 或其他更新檔案，不是.msi、.msu 或.msp 檔，例如，非\-Microsoft 驅動程式、 韌體與 BIOS 更新。  
  
    每個非\-預設的檔案類型設定為`<Folder>`中的項目`<FolderRules>`項目。 `<Folder>` 元素的名稱屬性必須與將包含對應類型更新之 Hotfix 根資料夾中的資料夾名稱相同。 資料夾可以位於**CAUHotfix\_所有**資料夾或節點中\-特定資料夾。 例如，若已在 Hotfix 根資料夾中設定 *FolderRule1* ，請在 XML 檔案中設定下列元素，以定義該資料夾中之更新的安裝範本與結束條件。  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
下表說明 `<Template>` 屬性與可能的 `<ExitConditions>` 子元素。  
  
|`<Template>` 屬性|描述|  
|--------------------------|---------------|  
|`path`|`<Extension name>` 屬性中定義之檔案類型的安裝程式完整路徑。<br /><br />若要指定 Hotfix 根資料夾結構中之更新的路徑，請使用 `$update$`。|  
|`parameters`|`path`中指定之程式的必要或選擇性參數字串。<br /><br />若要指定 Hotfix 根資料夾結構中之更新檔案的路徑參數，請使用 `$update$`。|  
  
|`<ExitConditions>` 子元素|描述|  
|---------------------------------|---------------|  
|`<Success>`|定義一或多個指出所指定更新已順利完成的結束代碼。 這是必要的子元素。|  
|`<Success_RebootRequired>`|您可以定義一或多個結束代碼，指出所指定更新成功且節點必須重新啟動 <br>**注意：** (可省略) `<Folder>` 元素可以包含 `alwaysReboot` 屬性。 若已設定此屬性，它會指出此規則安裝的 Hotfix 是否傳回 `<Success>`中定義的其中一個結束代碼，它是解譯為 `<Success_RebootRequired>` 結束條件。|  
|`<Fail_RebootRequired>`|您可以定義一或多個結束代碼，指出所指定更新失敗且節點必須重新啟動|  
|`<AlreadyInstalled>`|您可以定義一或多個結束代碼，指出所指定更新因為已安裝而未套用。|  
|`<NotApplicable>`|您可以定義一或多個結束代碼，指出所指定更新因為不適用於叢集節點而未套用。|  
  
> [!IMPORTANT]  
> 未在 `<ExitConditions>` 中明確定義的所有結束代碼都會被解譯為更新失敗而且節點未重新啟動。  
  
### <a name="BKMK_ACL"></a>限制對 hotfix 根資料夾的存取  
您必須執行數個步驟來設定 SMB 檔案伺服器與檔案共用，協助保護 Hotfix 根資料夾檔案與 Hofix 設定檔，只允許在 **Microsoft.HotfixPlugin**的環境中進行存取。 這些步驟會啟用數個功能，它們可以協助防止 Hotfix 檔案遭竄改而使得容錯移轉叢集有安全之虞。  
  
一般步驟如下所示：  
  
1.  識別可供 「 更新執行使用隨插即用的使用者帳戶\-中  
  
2.  在 SMB 檔案伺服器上的適當群組中設定此使用者帳戶  
  
3.  設定對 Hotfix 根資料夾的存取權限  
  
4.  設定 SMB 資料完整性設定  
  
5.  在 SMB 伺服器上啟用 Windows 防火牆規則  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>步驟 1. 識別使用 hotfix 外掛程式更新執行 」 使用的使用者帳戶\-中
  
在 CAU 中用來檢查執行 「 更新執行使用時的安全性設定的帳戶**Microsoft.HotfixPlugin**視 CAU 使用遠端\-更新模式或 self\-更新模式中，作為如下所示：  
  
-   **遠端\-更新模式**預覽及套用更新叢集具有系統管理權限的帳戶。  
  
-   **自助\-更新模式**叢集角色針對 CAU Active Directory 中設定之虛擬電腦物件的名稱。 這是針對 CAU 叢集角色在 Active Directory 中預先設置之虛擬電腦物件的名稱，或 CAU 針對叢集角色產生的名稱。 若要取得由 CAU 產生的名稱，請執行**取得\-CauClusterRole** CAU PowerShell cmdlet。 在輸出中，**ResourceGroupName** 是產生之虛擬電腦物件帳戶的名稱。  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>步驟 2. 在 SMB 檔案伺服器上的適當群組中設定此使用者帳戶
  
> [!IMPORTANT]  
> 您必須在 SMB 伺服器上新增一個帳戶，供「更新執行」做為本機系統管理員帳戶使用。 若因為您組織的安全性原則而不允許您這樣做，請使用下列程序在 SMB 伺服器上為此帳戶設定必要權限。  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>在 SMB 伺服器上設定使用者帳戶  
  
1.  新增將供「更新執行」使用的帳戶至 Distributed COM Users 群組與下列其中一個群組：Power User、Server Operation 或 Print Operator。  
  
2.  若要為該帳戶啟用必要的 WMI 權限，請啟動 SMB 伺服器上的 WMI 管理主控台。 啟動 PowerShell，然後輸入下列命令：  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  在主控台樹狀目錄中，以滑鼠右鍵\-按一下  **WMI 控制\(本機\)**，然後按一下**屬性**。  
  
4.  按一下 [安全性] ，然後展開 [根目錄] 。  
  
5.  按一下 [CIMV2] ，然後按一下 [安全性] 。  
  
6.  新增將供「更新執行」使用的帳戶至 [群組或使用者名稱] 清單。  
  
7.  將「執行方法」與「遠端啟用」權限授與將供「更新執行」使用的帳戶。  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>步驟 3。 設定對 Hotfix 根資料夾的存取權限
  
根據預設，當您嘗試套用更新，hotfix 插入\-中檢查 存取 hotfix 根資料夾的 NTFS 檔案系統權限的設定。 如果資料夾的存取權限設定不正確，「 更新執行使用 hotfix 外掛程式\-中可能會失敗。  
  
如果您使用 hotfix 外掛程式的預設組態\-中，請確定資料夾存取權限符合下列需求。  
  
-   Users 群組必須具有「讀取」權限。  
  
-   如果插頭\-中會將更新套用副檔名為.exe、 「 使用者 」 群組具有 Execute 權限。  
  
-   允許只有特定安全性主體\(但非必要\)寫入或修改權限。 允許的主體是本機 Administrators 群組、SYSTEM、CREATOR OWNER 與 TrustedInstaller。 其他帳戶或群組不能有 Hotfix 根資料夾的「寫入」或「修改」權限。  
  
（選擇性） 您可以停用先前的檢查，隨插即用\-中預設會執行。 若要執行這項作業，您可以使用兩種方法：  
  
-   如果您使用 CAU 的 PowerShell cmdlet，設定**DisableAclChecks\='True'** 中的引數**CauPluginArguments** hotfix 外掛程式的參數\-中。  
  
-   若您是使用 CAU UI，請在用來設定「更新執行」選項的精靈中，選取 [其他更新選項] 頁面上的 [停用 Hotfix 根資料夾和設定檔的管理員存取權檢查] 選項。  
  
不過，做為許多環境中的最佳做法，我們建議您使用預設設定來強制這些檢查。  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>步驟 4. 設定 SMB 資料完整性設定
  
若要檢查叢集節點與 SMB 檔案共用之間的連線中的資料完整性，hotfix 插入\-中，您必須啟用 SMB 簽署或 SMB 加密的 SMB 檔案共用上的設定。 支援 SMB 加密，它提供增強的安全性和更佳的效能，在許多環境中，從 Windows Server 2012 開始。 您可以啟用任一設定或同時啟用這兩個設定，如下所示：  
  
-   若要啟用 SMB 簽署，請參閱 Microsoft 知識庫中的 [文章 887429](https://support.microsoft.com/kb/887429) 。  
  
-   若要為 SMB 共用資料夾啟用 SMB 加密，請在 SMB 伺服器上執行下列 PowerShell cmdlet:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    其中，<*ShareName*> 是 SMB 共用資料夾的名稱。  
  
（選擇性） 若要強制使用 SMB 加密的 SMB 伺服器的連線中，選取**中的 存取 hotfix 根資料夾時需要 SMB 加密**選項在 CAU UI 中，或設定**RequireSMBEncryption\='True'** 插入\-中使用 CAU 的 PowerShell cmdlet 的引數。  
  
> [!IMPORTANT]  
> 若選取強制使用 SMB 加密的選項，且未針對使用 SMB 加密的連線設定 Hotfix 根資料夾，「更新執行」將失敗。  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>步驟 5. 在 SMB 伺服器上啟用 Windows 防火牆規則
  
您必須啟用**檔案伺服器遠端管理\(SMB\-中\)** SMB 檔案伺服器上的 Windows 防火牆規則。 這會啟用 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 中的預設值。  
  
## <a name="see-also"></a>另請參閱  
  
-   [叢集感知更新概觀](cluster-aware-updating.md)
  
-   [叢集感知更新的 Windows PowerShell Cmdlet](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [叢集感知更新外掛程式參考](https://msdn.microsoft.com/library/hh418084.aspx)  
  
