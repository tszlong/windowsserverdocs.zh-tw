---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: "叢集更新插的運作方式"
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
ms.technology: storage-failover-clustering
description: "如何使用插中使用時叢集更新 Windows 伺服器叢集上安裝的更新座標的更新。"
ms.openlocfilehash: eb606dfbe6596ecabd35e6ac36624fab2b4436b9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>叢集更新插的運作方式

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

[叢集更新](cluster-aware-updating.md)(CAU) 使用插上容錯移轉叢集節點協調安裝的更新。 本主題提供使用 built\ 中 CAU plug\ 單元或 CAU 您安裝其他 plug\ 集的相關資訊。

## <a name="BKMK_INSTALL"></a>安裝 plug\ 中  
Plug\ 中以外預設 plug\ 單元已安裝的 CAU \ (**Microsoft.WindowsUpdatePlugin**和**Microsoft.HotfixPlugin**\) 必須安裝另行購買。 如果 CAU 用於 self\ 更新模式時，plug\ 中必須安裝所有叢集節點。 如果 CAU 用於 remote\ 更新模式時，plug\ 中必須安裝更新協調器遠端電腦上。 Plug\ 在您安裝可能需要其他安裝需求每個節點上。  
  
安裝 plug\ 中，請依照下列指示 plug\ 中發行者。 若要手動與 CAU 登記 plug\ 中，執行[進行登記-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet plug\ 中安裝所在的每一部電腦上。  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>指定 plug\ 在並 plug\ 引數  
  
### <a name="specify-a-cau-plug-in"></a>指定 CAU plug\ 中

在 CAU UI，您 plug\ 中從清單中選取 drop\ 下的增益 plug\ 當您使用 CAU 執行下列動作：  
  
-   適用於叢集更新  
  
-   用於叢集 preview 更新  
  
-   設定叢集 self\ 更新選項  
  
根據預設，CAU 選取 plug\ 在**Microsoft.WindowsUpdatePlugin**。 不過，您可以指定任何 plug\ 中的安裝和使用 CAU 登記完畢。

> [!TIP]  
> 在 CAU UI，您可以僅限指定單一 plug\ 單元 CAU 使用預覽或更新執行期間套用更新。 藉由使用 CAU PowerShell cmdlet，您可以指定一或多個 plug\ 增益集。如果您需要叢集上安裝的更新多個類型，執行一個，請更新多個 plug\ 單元指定通常更有效率而不是使用不同的每個執行更新 plug\ 中。 例如，通常會發生較少節點重新開機。

使用下表列出的 CAU PowerShell cmdlet，您可以指定一或多個 plug\ 單元對更新執行或掃描傳遞**– CauPluginName**的參數。 您可以指定 plug\ 中名稱多個以逗號分隔。 若您指定了多個 plug\ 單元，您也可以控制如何 plug\ 單元影響彼此更新執行期間藉由**\-RunPluginsSerially**，**\-StopOnPluginFailure**，並**– SeparateReboots**的參數。 如需有關如何使用多個 plug\ 集的詳細資訊，請使用下表中的 cmdlet 文件所提供的連結。  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|將提供 self\ 更新功能，以指定叢集 CAU 叢集的角色。|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|執行的掃描叢集節點適用的更新，並透過指定叢集上更新執行這些更新會安裝。|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|執行的掃描叢集節點適用的更新，並傳回會套用指定的叢集中的每個節點更新的初始設定的清單。|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|設定 CAU 叢集角色組態屬性，指定叢集上。|  
  
如果您未使用下列 cmdlet 來指定 CAU plug\ 中參數，預設值是 plug\ 在**Microsoft.WindowsUpdatePlugin**。  
  
### <a name="specify-cau-plug-in-arguments"></a>指定 CAU plug\ 中引數  
當您設定的更新執行選項時，您可以指定一或多*name\ = 值*配對 \(arguments\) 選取 plug\-中使用。 例如，CAU ui，您可以指定多個引數如下：  
  
**Name1\ = Value1;Name2\ = Value2;Name3\ = Value3**  
  
這些*name\ = 值*配對必須 plug\ 中的意義，指定。 針對某些 plug\ 單元引數是選擇性的。  
  
引數 CAU plug\ 中的語法遵循這些一般規則：  
  
-   多個*name\ = 值*分號來分隔配對。  
  
-   含有空間值住引號，例如：**Name1\ =「值空間]**。  
  
-   確切語法*值*上 plug\ 中而有所不同。  
  
使用 CAU PowerShell cmdlet 支援指定 plug\ 中引數**– CauPluginParameters**參數，pass 表單的參數為：  
  
**\-CauPluginArguments @{Name1\ = Value1;Name2\ = Value2;Name3\ = Value3}**  
  
您也可以使用預先定義的 PowerShell hash 資料表。 若要指定 plug\ 中引數一個以上的 plug\ 中傳遞多個以逗號分隔的引數 hash 表格。 順序 plug\ 中指定傳遞 plug\ 引數**CauPluginName**。  
  
### <a name="specify-optional-plug-in-arguments"></a>指定選擇性 plug\ 中引數  
安裝 CAU plug\ 單元 \ (**Microsoft.WindowsUpdatePlugin**和**Microsoft.HotfixPlugin**\) 提供其他選項，您可以選取。 在 CAU UI，這些出現在**其他選項**頁面上之後您設定 plug\ 中對更新執行的選項。 如果您使用 CAU PowerShell cmdlet、下列選項設定為選擇性 plug\ 中引數。 如需詳細資訊，請查看[使用 Microsoft.WindowsUpdatePlugin](#BKMK_WUP)和[使用 Microsoft.HotfixPlugin](#BKMK_HFP)本主題中的更新版本。  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>管理單元 plug\ 使用 Windows PowerShell cmdlet  
  
|Cmdlet|描述|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|擷取一或多個更新 plug\ 增益集，登記本機電腦上的軟體的相關資訊。|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|暫存器 CAU 軟體更新 plug\ 入本機電腦上。|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|軟體更新 plug\ 單元從 plug\ 增益集，可供 CAU 的清單中移除。 **注意：**安裝的 CAU plug\ 單元 \ (**Microsoft.WindowsUpdatePlugin**和**Microsoft.HotfixPlugin**\) 無法取消。|  
  
## <a name="BKMK_WUP"></a>使用 Microsoft.WindowsUpdatePlugin  

預設的 plug\ 單元 CAU，**Microsoft.WindowsUpdatePlugin**，執行下列動作：
- 使用 Windows Update 代理程式每個容錯移轉叢集節點上套用更新所需的每個節點執行 Microsoft 你會通訊。
- 直接從 Windows Update 或 Microsoft Update，或 on\ 先 Windows Server Update Services \(WSUS\) 伺服器，請安裝叢集更新。
- 僅選取，安裝一般 distribution 發行 \(GDR\) 更新。 根據預設，plug\ 在適用於僅適用的重要軟體更新。 不不需要任何設定。 預設設定，下載並安裝重要 GDR 更新每個節點上。 

> [!NOTE]
> 若要套用更新以外的重要軟體更新選取預設的 \（例如，驅動程式 updates\），您可以設定為選擇性 plug\ 中參數。 如需詳細資訊，請查看[設定 Windows 更新代理程式查詢字串](#BKMK_QUERY)。

### <a name="requirements"></a>需求

- 遠端更新協調器電腦 \(if used\) 與容錯移轉叢集必須符合的需求 CAU 和設定所需的遠端管理列在[需求與最佳做法 CAU](cluster-aware-updating-requirements.md)。
- 檢視[適用於套用更新 Microsoft 建議](cluster-aware-updating-requirements.md#BKMK_BP_WUA)，然後容錯移轉叢集節點您 Microsoft Update 設定來進行任何必要變更。
- 取得最佳效果，我們建議您執行的最佳做法分析 CAU \(BPA\) 確保叢集和更新環境正確設定使用 CAU 套用更新。 如需詳細資訊，請查看[更新整備測試 CAU](cluster-aware-updating-requirements.md#BKMK_BPA)。

> [!NOTE]
> 排除需要接受 Microsoft 授權合約，或者需要與使用者互動的更新，以及他們必須手動安裝。

### <a name="additional-options"></a>其他選項

或者，您可以指定下列 plug\ 中引數擴大或限制的更新 plug\ 中所套用的設定：
- Plug\ 中上套用除了每個節點，在 CAU UI 上的重要更新建議的更新設定**的其他選項**頁面上，選取**提供建議更新與接收重要更新的方式相同**核取方塊。
<br>或者，請設定**'IncludeRecommendedUpdates' \' true '**中 plug\ 引數。
- 若要設定 plug\ 中篩選適用於叢集的每個節點 GDR 更新的類型，指定 Windows Update 代理程式查詢字串使用**查詢**中 plug\ 引數。 如需詳細資訊，請查看[設定 Windows 更新代理程式查詢字串](#BKMK_QUERY)。

### <a name="BKMK_QUERY"></a>設定 Windows 更新代理程式查詢字串  
您可以設定 plug\ 中引數 plug\ 中的預設的**Microsoft.WindowsUpdatePlugin**，可包含 Windows Update 代理程式 \(WUA\) 查詢字串。 這個指令使用 WUA API 找出的 Microsoft 更新適用於特定選取條件為基礎的每個節點一或多個群組。 您可以使用邏輯，或 OR 邏輯結合多個條件。 WUA 查詢字串中指定 plug\ 中引數，如下所示：  
  
**QueryString\ =」Criterion1\ = Value1 and\ 日或 Criterion2\ = Value2 and\ 日或...」**  
  
例如**Microsoft.WindowsUpdatePlugin**會自動選取 [使用預設的 [重要更新**查詢**使用建構引數**已安裝**、**輸入**、**IsHidden**，和**IsAssigned**條件：  
  
**QueryString\ =」IsInstalled\ = 0 和 Type\ = '軟體' 和 IsHidden\ = 0 和 IsAssigned\ = 1 台」**  
  
若您指定**查詢**引數，它用來取代預設的**查詢**plug\ 中的設定。  
  
#### <a name="example-1"></a>範例 1
  
若要設定**查詢**安裝特定的更新，以 ID 引數*f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\ =」UpdateID\ ='f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' 和 IsInstalled\ = 0」**  
  
> [!NOTE]  
> 前一個範例是適用於使用 Cluster\ 感知更新精靈套用更新。 如果您想要安裝特定的更新，藉由設定 self\ 更新選項 CAU UI 或使用**Add\-CauClusterRole**或**Set\-CauClusterRole**PowerShell cmdlet，您必須格式化具有兩個 single\ 引號字元 UpdateID 值：  
>   
> **QueryString\ =」UpdateID\ = f6ce46c1\-971c\-43f9\-a2aa\-783df125f003 ' 和 IsInstalled\ = 0」**  
  
#### <a name="example-2"></a>範例 2
  
若要設定**查詢**，將只驅動程式安裝引數：  
  
**QueryString\ =」IsInstalled\ = 0 和 Type\ = '驅動程式' 和 IsHidden\ = 0」**  
  
如需有關查詢字串 plug\ 中的預設的**Microsoft.WindowsUpdatePlugin**，搜尋條件 \ (例如**已安裝**\)，，您可以在查詢字串，包括語法看到的區段，搜尋條件中相關[Windows Update 代理 (WUA) API 參考](http://go.microsoft.com/fwlink/p/?LinkId=223304)。  
  
## <a name="BKMK_HFP"></a>使用 Microsoft.HotfixPlugin  
Plug\ 在**Microsoft.HotfixPlugin**可以用來適用於 Microsoft 有限 distribution 版本 \(LDR\) 更新 \（也稱為 hotfix、和先前稱為 QFEs\），則您獨立下載地 Microsoft 軟體的特定問題。 外掛程式從根 SMB 檔案共用資料夾安裝的更新，您也可以自訂套用 non\ 的 Microsoft 驅動程式、韌體和 BIOS 更新。

> [!NOTE]
> Hotfix 有時候可供下載 Microsoft 知識庫文章中，但也提供針對 as\ 需要為基礎。

### <a name="requirements"></a>需求

- 遠端更新協調器電腦 \(if used\) 與容錯移轉叢集必須符合的需求 CAU 和設定所需的遠端管理列在[需求與最佳做法 CAU](cluster-aware-updating-requirements.md)。
- 檢視[適用於使用 Microsoft.HotfixPlugin 建議](cluster-aware-updating-requirements.md#BKMK_BP_HF)。
- 取得最佳效果，我們建議您執行的最佳做法分析 CAU \(BPA\) 型號確保叢集和更新環境正確設定使用 CAU 套用更新。 如需詳細資訊，請查看[更新整備測試 CAU](cluster-aware-updating-requirements.md#BKMK_BPA)。
- 從「發行者」取得更新，將它們複製或將它們解壓縮至伺服器訊息區 \(SMB\) 檔案共用 \ (hotfix 根 folder\) 的支援至少 SMB 2.0 和，都可以存取所有叢集節點協調更新器遠端電腦 \（如果 CAU 用於 remote\ 更新 mode\）。 如需詳細資訊，請查看[設定 hotfix 根資料夾結構](#BKMK_HF_ROOT)本主題中的更新版本。 

    > [!NOTE]
    > 根據預設，這 plug\ 單元只安裝 hotfix 具有下列副檔名：.msu、.msi，以及.msp。

- 複製檔案 DefaultHotfixConfig.xml \ (中提供的**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating**資料夾的電腦上的 CAU 工具 installed\) 您到您所建立的 hotfix 根資料夾和所在解壓縮 hotfix。 例如，設定將檔案複製到*\\\MyFileServer\\Hotfixes\\Root\\*。 

    > [!NOTE]
    > 若要安裝最 hotfix 提供 Microsoft 和其他的更新，預設 hotfix 設定檔可用修改而。 如果您的案例需要它，您可以為進階任務自訂設定檔。 設定檔可以包含自訂規則，處理 hotfix 檔案有特定的延伸模組或來定義特定結束條件的行為。 如需詳細資訊，請查看[自訂 hotfix 設定檔](#BKMK_CONFIG_FILE)本主題中的更新版本。

### <a name="configuration"></a>設定

下列設定。 如需詳細資訊，查看稍後在本主題中的區段的連結。
- 共用的 hotfix 根資料夾，其中包含套用更新和的路徑包含 hotfix 設定檔。 您可以輸入此路徑 CAU UI 或設定**HotfixRootFolderPath\ = \ < 路徑 >** PowerShell plug\ 中引數。 

   > [!NOTE]
   > 您可以指定 hotfix 根資料夾路徑本機的資料夾，或為底色表單的*\\\ServerName\\Share\\RootFolderName*。 可用 domain\ 型或獨立 DFS 命名空間路徑。 不過，檢查 hotfix 設定檔中的存取權限的相容 DFS 命名空間路徑，所以如果您設定一個，您必須停用檢查是否有 plug\ 中功能存取權限使用 CAU UI 或設定**DisableAclChecks\' true '**中 plug\ 引數。
- 檢查的適當權限存取資料夾，並確定從 SMB 存取資料的完整性 hotfix 根資料夾的伺服器上的設定共用資料夾 \ (SMB 登入或 SMB Encryption\)。 如需詳細資訊，請查看[限制 hotfix 根資料夾的存取](#BKMK_ACL)。

### <a name="additional-options"></a>其他選項

- （選擇性）設定 plug\ 中，SMB 加密執行時存取 hotfix 檔案共用的資料。 在 CAU UI，在**其他選項**頁面上，選取**中存取 hotfix 根資料夾需要 SMB 加密**選項，或設定**RequireSMBEncryption\' true '** PowerShell plug\ 中引數。 
  > [!IMPORTANT]
  > 您必須執行額外的設定步驟，可讓 SMB SMB 登入或 SMB 加密資料的完整性 SMB 伺服器上。 如需詳細資訊，請查看中執行「步驟 4[限制 hotfix 根資料夾的存取](#BKMK_ACL)。 如果您選擇使用 SMB 加密，並 hotfix 根資料夾執行無法存取使用 SMB 加密，在更新文字將會失敗。
- （選擇性）停用預設檢查不足的權限 hotfix 根資料夾和 hotfix 設定檔。 在 CAU UI，選取 [**停用檢查 hotfix 根資料夾和設定檔系統管理員權限的**，或設定**DisableAclChecks\' true '** plug\ 中引數。
- （選擇性）設定**HotfixInstallerTimeoutMinutes\ =<Integer>**引數指定多久 plug\ 中等待退貨 hotfix 安裝程序。 \ (預設值為 30 分鐘的時間。\)，例如指定逾時兩個小時的時間，請設定**HotfixInstallerTimeoutMinutes\ = 120**。
- （選擇性）設定**HotfixConfigFileName \ = <name>**中 plug\ 引數指定位於 hotfix 根資料夾 hotfix 設定檔的名稱。 如果未指定，會使用預設的名稱 DefaultHotfixConfig.xml。
  
### <a name="BKMK_HF_ROOT"></a>設定 hotfix 根資料夾結構

Plug\ 中的工作，hotfix 必須儲存在 SMB 檔案共用 well\ 定義結構 \ (hotfix 根 folder\)，您必須使用 CAU UI 或 CAU PowerShell cmdlet hotfix 根資料夾的路徑與設定 plug\。 這個路徑傳送到 plug\ 為**HotfixRootFolderPath**引數。 您可以選擇數個結構 hotfix 根資料夾中的其中一個更新您的需求，依據以下的範例所示。 忽略檔案或資料夾不符合結構。  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>範例 1-資料夾結構用來套用到所有叢集節點 hotfix
  
若要指定 hotfix 適用於所有叢集節點，將它們複製到名為**CAUHotfix\_All**下方的 hotfix 根資料夾。 在此範例中，**HotfixRootFolderPath**設定為在 plug\ 引數*\\\MyFileServer\\Hotfixes\\Root\\*。 **CAUHotfix\_All**資料夾包含.msu 擴充功能、與.msi，這會套用到所有叢集節點.msp 三個更新。 更新檔案名稱是僅針對圖用途。  
  
> [!NOTE]  
> 在這及以下的範例，以預設名稱 DefaultHotfixConfig.xml hotfix 設定檔所示它的必要位置 hotfix 根資料夾中。  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>用於特定的更新只適用於特定節點範例 2-結構資料夾
  
若要指定 hotfix 僅適用於特定節點，使用 hotfix 根資料夾節點的名稱下方的子資料夾。 使用叢集] 節點 NetBIOS 名稱，例如*ContosoNode1*。 然後，將更新僅適用於此節點此子資料夾。 下列範例中，**HotfixRootFolderPath**設定為在 plug\ 引數*\\\MyFileServer\\Hotfixes\\Root\\*。 在更新**CAUHotfix\_All**資料夾會套用到所有叢集節點，和*Node1\_Specific\_Update.msu*僅會套用*ContosoNode1*。  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>範例 3-資料夾結構用來套用以外.msu、.msi，以及.msp 檔案更新
  
根據預設，**Microsoft.HotfixPlugin**僅適用於.msu、.msi 或.msp 擴充功能的更新。 不過，可能會有不同的擴充功能特定的更新，並需要其他安裝的命令。 例如，您可能需要適用於叢集節點.exe 擴充功能的韌體更新。 您可以使用子資料夾，表示您有一個特定的設定 hotfix 根資料夾，應該要安裝 non\ 預設的更新類型。 您還必須設定指定中的安裝命令的對應資料夾安裝規則`<FolderRules>`hotfix 組態 XML 檔案中的項目。  
  
下列範例中，**HotfixRootFolderPath**設定為在 plug\ 引數*\\\MyFileServer\\Hotfixes\\Root\\*。 數個的更新將會套用到所有叢集節點，以及韌體更新*SpecialHotfix1.exe*將會套用到*ContosoNode1*來使用*FolderRule1*。 設定的相關資訊的*FolderRule1*在 hotfix 設定檔，會看到[自訂 hotfix 設定檔](#BKMK_CONFIG_FILE)本主題中的更新版本。  
  
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
Hotfix 設定檔控制項如何**Microsoft.HotfixPlugin**容錯移轉叢集中安裝特定 hotfix 檔案類型。 在 HotfixConfigSchema.xsd，位於下列電腦上的資料夾位置 CAU 工具安裝定義 XML 架構設定檔：  
  
**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating 資料夾**  
  
若要自訂 hotfix 設定檔，此位置範例設定檔 DefaultHotfixConfig.xml 複製到 hotfix 根資料夾以適當的修改案例。  
  
> [!IMPORTANT]  
> 適用於大多數 hotfix 提供 Microsoft 和其他的更新，以預設 hotfix 設定檔可用修改。 自訂的設定檔 hotfix 是進階的使用量案例中，只有工作。  
  
根據預設，hotfix 組態 XML 檔案定義安裝規則及以下兩種類型的 hotfix 結束條件：  
  
-   與 plug\ 中安裝預設的延伸 Hotfix 檔案 \ (.msu、.msi，以及.msp files\)。  
  
    這些定義為`<ExtensionRules>`中的項目`<DefaultRules>`的項目。 還有一個`<Extension>`的預設支援的檔案類型的每個項目。 一般 XML 結構如下：  
  
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
  
    如果您需要在您的環境中的所有叢集節點適都用於特定的更新類型，您可以定義其他`<Extension>`的項目。  
  
-   Hotfix 或其他更新檔案不會.msi、.msu 或.msp 檔案，例如 non\ 的 Microsoft 驅動程式、韌體和 BIOS 更新。  
  
    每個預設 non\ 檔案類型設定為`<Folder>`中的項目`<FolderRules>`的項目。 名稱屬性的`<Folder>`的項目必須是相同的資料夾中將包含更新相對應的類型的 hotfix 根資料夾的名稱。 資料夾可在**CAUHotfix\_All**資料夾或 node\ 特定資料夾中。 例如，如果*FolderRule1*是設定 hotfix 根資料夾中，設定下列項目 XML 檔案定義安裝範本和結束條件該資料夾中的更新中：  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
下表描述`<Template>`屬性，可能的`<ExitConditions>`子元素。  
  
|`<Template>` 屬性|描述|  
|--------------------------|---------------|  
|`path`|定義中的檔案類型的安裝程式的完整路徑`<Extension name>`屬性。<br /><br />若要指定 hotfix 根資料夾結構更新檔案的路徑，使用`$update$`。|  
|`parameters`|必要和選擇性的參數程式中所指定的字串`path`。<br /><br />若要指定 hotfix 根資料夾結構更新檔案的路徑的參數，使用`$update$`。|  
  
|`<ExitConditions>` 子元素|描述|  
|---------------------------------|---------------|  
|`<Success>`|定義一或多個結束代碼，表示成功指定的更新。 這是必要的子元素。|  
|`<Success_RebootRequired>`|或者定義一或多個結束代碼，表示成功指定的更新，以及節點必須重新開機。 <br>**注意：**（選擇性）`<Folder>`的項目可以包含`alwaysReboot`屬性。 此屬性設定，如果它表示如果安裝此規則 hotfix 傳回其中結束代碼中所定義`<Success>`、解譯為`<Success_RebootRequired>`結束條件。|  
|`<Fail_RebootRequired>`|或者定義指出指定的更新失敗，且節點必須重新開機一或多個結束驗證碼。|  
|`<AlreadyInstalled>`|或者定義一或多個結束代碼，表示它已安裝，所以不套用指定的更新。|  
|`<NotApplicable>`|或者定義一或多個結束代碼，表示無法套用指定的更新，因為它並不適用於叢集節點。|  
  
> [!IMPORTANT]  
> 任何結束中未明確定義的程式碼`<ExitConditions>`更新失敗，並不會重新開機] 節點解譯。  
  
### <a name="BKMK_ACL"></a>只存取 hotfix 根資料夾  
您必須執行設定 SMB 檔案伺服器和檔案分享來協助保護 hotfix 根資料夾的檔案和 hofix 存取只的部分的設定檔的幾個步驟**Microsoft.HotfixPlugin**。 幾個功能，以避免可能竄改 hotfix 檔案可能危害容錯移轉叢集的方式可讓這些步驟。  
  
一般步驟如下：  
  
1.  找出使用者 account 用於使用 plug\ 中更新執行  
  
2.  此使用者 account 設定 SMB 檔案伺服器上的必要群組中  
  
3.  設定 hotfix 根資料夾的存取權限  
  
4.  設定 SMB 資料的完整性  
  
5.  讓 Windows 防火牆 SMB 伺服器規則  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>步驟 1。 找出使用者 account 用於使用 plug\ 中更新執行
  
檢查安全性設定同時執行更新執行使用用於 CAU account **Microsoft.HotfixPlugin**會隨著是否 CAU 使用 remote\ 更新模式或 self\ 更新模式下，如下所示：  
  
-   **Remote\ 更新模式**account 預覽，並套用更新叢集上的系統管理員權限。  
  
-   **Self\ 更新模式**設定 Active Directory 中為 CAU virtual 電腦物件的名稱叢集角色。 這是在 Active Directory 中為 CAU 叢集角色預備 virtual 電腦物件的名稱或由叢集角色 CAU 的名稱。 若要取得由 CAU 名稱、執行**Get\-CauClusterRole** CAU PowerShell cmdlet。 在輸出中，**ResourceGroupName**是產生 virtual 電腦物件 account 的名稱。  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>步驟 2。 此使用者 account 設定 SMB 檔案伺服器上的必要群組中
  
> [!IMPORTANT]  
> 您必須將新增對更新執行使用本機系統管理員 account SMB 伺服器上為 account。 如果不允許這是因為您在組織中的安全性原則，使用下列程序此 account 設定必要 SMB 伺服器上的權限的。  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>設定使用者 account SMB 伺服器上  
  
1.  新增到散發 COM Users 群組並下列群組用於更新執行 account：進階使用者、伺服器作業或列印電信業者。  
  
2.  若要讓必要的權限 WMI 帳號，開始 WMI 管理主控台 SMB 伺服器。 PowerShell [開始]，然後輸入下列命令：  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  主控台中 right\ 按一下**WMI 控制 \(Local\)**，然後按一下 [**屬性**。  
  
4.  按一下**安全性**，然後展開**根**。  
  
5.  按一下**CIMV2**，然後按**的安全性**。  
  
6.  新增適用於更新執行到帳號**群組或使用者名稱**清單中。  
  
7.  授與**執行方法**和**可讓遠端**更新執行用於 account 權限。  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>步驟 3。 設定 hotfix 根資料夾的存取權限
  
根據預設，當您嘗試套用更新 plug\ 中檢查存取 hotfix 根資料夾的 NTFS 檔案系統權限的設定。 如果資料夾的存取權限設定不正確，可能會失敗更新執行使用 plug\。  
  
如果您使用 plug\ 中的預設設定，請確認資料夾的存取權限符合下列需求。  
  
-   Users 群組具有讀取權限。  
  
-   如果 plug\ 中將會套用.exe 擴充功能的更新，請 Users 群組已執行權限。  
  
-   允許只有特定的安全性原則 \（但不是 required\）有寫入或修改權限。 允許的主體是本機系統管理員群組，系統、CREATOR 擁有者，以及 TrustedInstaller。 其他帳號或群組不允許寫入或修改權限 hotfix 根資料夾。  
  
或者，您可以停用 plug\ 中執行預設的上述檢查。 您可以執行下列其中一種方式：  
  
-   如果您使用 CAU PowerShell cmdlet、已設定**DisableAclChecks\' true '**中的引數**CauPluginArguments** plug\ 中的參數。  
  
-   如果您使用 CAU UI，選取 [**停用檢查 hotfix 根資料夾和設定檔系統管理員權限的**選項，在**更新的其他選項**精靈用來設定更新執行選項] 頁面。  
  
不過，做為最佳做法，您的環境中，我們建議您在執行這些檢查使用預設設定。  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>步驟 4。 設定 SMB 資料的完整性
  
若要檢查在間叢集節點和 SMB 檔案共用資料的完整性，plug\ 中要求您可以設定 SMB SMB 登入或 SMB 加密檔案共用。 SMB 加密，可提供提高的安全性與您的環境中更好的效能，支援 Windows Server 2012 中開始。 您可以讓下列其中一個或兩個這些設定，如下所示：  
  
-   若要讓 SMB 登入，查看中的程序[文章 887429](http://support.microsoft.com/kb/887429) Microsoft 知識庫中。  
  
-   若要讓 SMB 加密 SMB 共用資料夾，執行下列 PowerShell cmdlet SMB 伺服器上：  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    位置 <*共用名稱*> [SMB 共用資料夾的名稱。  
  
（選擇性）若要執行的連接 SMB 伺服器 SMB 加密的使用，選取 [**存取 hotfix 根資料夾需要 SMB 加密**選項中 CAU UI，或設定**RequireSMBEncryption\' true '**中 plug\ 引數使用 CAU PowerShell cmdlet。  
  
> [!IMPORTANT]  
> 如果您選擇使用 SMB 加密，並 hotfix 根資料夾執行並未設定為使用 SMB 加密的連接，在更新文字將會失敗。  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>步驟 5。 讓 Windows 防火牆 SMB 伺服器規則
  
您必須支援**檔案 Server 的遠端管理 \(SMB\-in\)**中的規則 Windows 防火牆 SMB 檔案伺服器上。 這是在 Windows Server 2016、Windows Server 2012 R2，以及 Windows Server 2012 預設支援。  
  
## <a name="see-also"></a>也了  
  
-   [叢集更新概觀](cluster-aware-updating.md)
  
-   [叢集更新的 Windows PowerShell Cmdlet](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [叢集更新外掛程式參考資料](http://msdn.microsoft.com/library/hh418084.aspx)  
  
