---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: 叢集感知更新外掛程式的工作方式
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: 在 Windows Server 中使用叢集感知更新以在叢集上安裝更新時，如何使用外掛程式來協調更新。
ms.openlocfilehash: 5fabd55f54527a2396643cea48980077891e3281
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948070"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>叢集感知更新外掛程式的工作方式

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

叢集[感知更新](cluster-aware-updating.md)（CAU）會使用外掛程式來協調容錯移轉叢集中各節點的更新安裝。 本主題提供有關使用 CAU 外掛程式\-的內建\-或您為 CAU 安裝的其他外掛程式\-的資訊。

## <a name="BKMK_INSTALL"></a>在中安裝外掛程式\-  
除了在與 CAU \(**microsoft.windowsupdateplugin**和**microsoft.hotfixplugin**\) 一起安裝的預設外掛程式\-外，還必須分別安裝外掛程式\-。 如果在自我\-更新模式中使用 CAU，則中的外掛程式\-必須安裝在所有叢集節點上。 如果在遠端\-更新模式中使用 CAU，則中的外掛程式\-必須安裝在遠端更新協調器電腦上。 您安裝的外掛程式\-在每個節點上可能會有額外的安裝需求。  
  
若要在中安裝「外掛程式」\-，請依照「發行者」中的「外掛程式\-」中的指示進行。 若要使用 CAU 手動註冊外掛程式\-，請在安裝外掛程式\-的每部電腦上執行[unregister-cauplugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) Cmdlet。  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>指定插入\-，並在引數中插入\-  
  
### <a name="specify-a-cau-plug-in"></a>在中指定 CAU 外掛程式\-

在 CAU UI 中，當您使用 CAU 來執行下列動作時，您可以從\-[可用的外掛程式] 下拉清單中選取 [插入\-]\-：  
  
-   套用更新至叢集  
  
-   預覽叢集的更新  
  
-   設定叢集自我\-更新選項  
  
根據預設，CAU 會選取**microsoft.windowsupdateplugin**中的 [外掛程式]\-。 不過，您可以在中指定已安裝並向 CAU 註冊的任何外掛程式\-。

> [!TIP]  
> 在 CAU UI 中，您只能指定中的單一插頭\-，讓 CAU 用來預覽或在「更新執行」期間套用更新。 藉由使用 CAU PowerShell Cmdlet，您可以指定一或多個「外掛程式」\-。如果您需要在叢集上安裝多個類型的更新，在一個「更新執行」中指定多個「外掛程式」\-，而不是針對中的每個「外掛程式」\-使用個別的「更新執行」，通常會更有效率。 例如，通常需要重新啟動的節點數目較少。

藉由使用下表所列的 CAU PowerShell Cmdlet，您可以藉由傳遞 **– CauPluginName**參數，為更新執行或掃描指定一或多個外掛程式\-。 您可以使用逗號分隔，以指定名稱中的多個外掛程式\-。 如果您指定多個外掛程式\-，您也可以在「更新執行」期間，藉由指定 **\-RunPluginsSerially**、 **\-StopOnPluginFailure**和 **– SeparateReboots**參數，來控制「外掛程式\-」在執行期間如何影響彼此。 如需使用多個外掛程式\-的詳細資訊，請使用下表中提供給 Cmdlet 檔的連結。  
  
|Cmdlet|說明|  
|----------|---------------|  
|[新增-Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/add-cauclusterrole)|新增 CAU 叢集角色，以提供自我\-更新功能至指定的叢集。|  
|[叫用-Get-caurun](https://docs.microsoft.com/powershell/module/clusterawareupdating/invoke-caurun)|執行叢集節點掃描以判斷適用的更新，並透過所指定叢集上的「更新執行」安裝那些更新。|  
|[叫用-Invoke-causcan](https://docs.microsoft.com/powershell/module/clusterawareupdating/invoke-causcan)|執行叢集節點掃描以判斷適用的更新，並傳回將套用至所指定叢集中每個節點的初始更新集清單。|  
|[設定-Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/set-cauclusterrole)|設定所指定叢集上的 CAU 叢集角色屬性。|  
  
如果您未使用這些 Cmdlet 在參數中指定 CAU 外掛程式\-，則預設值為**microsoft.windowsupdateplugin**中的插頭\-。  
  
### <a name="specify-cau-plug-in-arguments"></a>在引數中指定 CAU 外掛程式\-  
當您設定「更新執行」選項時，您可以指定一或多個*名稱\=值*組，\(引數\) 供中選取的插入\-使用。 例如，在 CAU UI 中，您可以指定多個引數，如下所示：  
  
**Name1\=Value1;Name2\=Value2;Name3\=Value3**  
  
這些*名稱\=值*組對您指定的外掛程式\-而言必須有意義。 對於某些外掛程式\-，引數是選擇性的。  
  
在引數中，CAU 外掛程式\-的語法會遵循下列一般規則：  
  
-   多個*名稱\=值*組會以分號分隔。  
  
-   包含空格的值會以引號括住，例如： **Name1\="含空格的值"** 。  
  
-   *Value*的確切語法取決於中的外掛程式\-。  
  
若要使用支援 **– CauPluginParameters**參數的 CAU PowerShell Cmdlet，在引數中指定插入\-，請傳遞下列格式的參數：  
  
**\-CauPluginArguments @ {Name1\=Value1;Name2\=Value2;Name3\=Value3}**  
  
您也可以使用預先定義的 PowerShell 雜湊表。 若要在中指定多個外掛程式\-的引數中的插入\-，請傳遞引數的多個雜湊表，並以逗號分隔。 以**CauPluginName**中指定的順序，在外掛程式\-的引數中傳遞插入\-。  
  
### <a name="specify-optional-plug-in-arguments"></a>在引數中指定選擇性的外掛程式\-  
CAU 安裝 \(**microsoft.windowsupdateplugin**和**microsoft.hotfixplugin**\) 提供其他選項，可供您選取。\- 在 CAU UI 中，這些會出現在 [**其他選項**] 頁面上（在中設定「外掛程式\-的「更新執行」選項之後）。 如果您使用 CAU PowerShell Cmdlet，這些選項會設定為引數中的選擇性外掛程式\-。 如需詳細資訊，請參閱本主題稍後的 [使用 Microsoft.WindowsUpdatePlugin](#BKMK_WUP) 和 [使用 Microsoft.HotfixPlugin](#BKMK_HFP) 。  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>使用 Windows PowerShell Cmdlet 管理外掛程式\-  
  
|Cmdlet|說明|  
|----------|---------------|  
|[Unregister-cauplugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/get-cauplugin)|抓取在本機電腦上註冊的一或多個軟體更新\-外掛程式的相關資訊。|  
|[註冊-Unregister-cauplugin]((https://docs.microsoft.com/powershell/module/clusterawareupdating/register-cauplugin))|在本機電腦上註冊 CAU 軟體更新外掛程式\-。|  
|[取消註冊-Unregister-cauplugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/unregister-cauplugin)|從可供 CAU 使用的外掛程式\-清單中移除軟體更新外掛程式\-。 **注意：** 無法取消註冊與 CAU \(**microsoft.windowsupdateplugin**和**microsoft.hotfixplugin**\) 一起安裝的外掛程式\-。|  
  
## <a name="BKMK_WUP"></a>使用 Microsoft.windowsupdateplugin  

**MICROSOFT.WINDOWSUPDATEPLUGIN**CAU 的預設插入\-，會執行下列動作：
- 與每個容錯移轉叢集節點上的「Windows Update 代理程式」通訊，以套用每個節點上執行之 Microsoft 產品所需的更新。
- 直接從 Windows Update 或 Microsoft Update 安裝叢集更新，或從 \(WSUS\) 伺服器上的\-部署 Windows Server Update Services。
- 只安裝選取的一般發行版本 \(GDR\) 更新。 根據預設，中的外掛程式\-只會套用重要的軟體更新。 不需要進行任何設定。 預設設定會在每個節點上下載並安裝重要 GDR 更新。 

> [!NOTE]
> 若要套用預設所選的重要軟體更新以外的更新 \(例如，\)的驅動程式更新，您可以在參數中設定選擇性的插頭\-。 如需詳細資訊，請參閱 [設定 Windows Update 代理程式查詢字串](#BKMK_QUERY)。

### <a name="requirements"></a>需求

- 容錯移轉叢集與遠端更新協調器電腦 \(如果使用\) 必須符合 CAU 的需求，以及在[cau 的需求和最佳作法](cluster-aware-updating-requirements.md)中所列的遠端系統管理所需的設定。
- 檢閱[套用 Microsoft 更新的建議](cluster-aware-updating-requirements.md#BKMK_BP_WUA)，接著針對容錯移轉叢集節點進行所需的任何 Microsoft Update 設定變更。
- 為了獲得最佳結果，我們建議您 \(BPA\) 執行 CAU 最佳做法分析程式，以確保叢集和更新環境已正確設定，以使用 CAU 來套用更新。 如需詳細資訊，請參閱 [Test CAU updating readiness](cluster-aware-updating-requirements.md#BKMK_BPA)。

> [!NOTE]
> 系統會排除需要接受 Microsoft 授權條款或需要使用者互動的更新，您需要手動安裝這些更新。

### <a name="additional-options"></a>其他選項

（選擇性）您可以在引數中指定下列的插入\-，以增強或限制中外掛程式\-所套用的更新集合：
- 若要在中設定「外掛程式」\-以套用建議的更新，以及每個節點上的重要更新，請在 CAU UI 的 [**其他選項**] 頁面上，選取 [**以我接收重要更新的方式提供建議的更新**] 核取方塊。
<br>或者，在引數中設定 **' IncludeRecommendedUpdates '\=' True** ' 外掛程式\-。
- 若要在中設定「外掛程式」\-，以篩選套用至每個叢集節點的 GDR 更新類型，請在引數中使用**QueryString**插入\-來指定 Windows Update 代理程式查詢字串。 如需詳細資訊，請參閱 [設定 Windows Update 代理程式查詢字串](#BKMK_QUERY)。

### <a name="BKMK_QUERY"></a>設定 Windows Update 代理程式查詢字串  
您可以在**microsoft.windowsupdateplugin**中設定預設插頭\-的插入\-，此引數是由 Windows Update 代理程式 \(WUA\) 查詢字串所組成。 此指示使用 WUA API 來識別要套用到每個節點的一或多個 Microsoft 更新群組 (根據特定選取範圍條件)。 您可以使用邏輯 AND 或邏輯 OR 來結合多個條件。 WUA 查詢字串是在引數的插入\-中指定，如下所示：  
  
**QueryString\="Criterion1\=Value1 and\/or Criterion2\=Value2 and\/or ..."**  
  
例如，**Microsoft.WindowsUpdatePlugin** 會使用預設的 **QueryString** 引數 (使用 **IsInstalled**、**Type**、**IsHidden** 與 **IsAssigned** 條件所建構) 自動選取重要更新。  
  
**QueryString\="IsInstalled\=0 並類型\=' Software ' 和 IsHidden\=0 and IsAssigned\=1"**  
  
如果您指定**QueryString**引數，則會使用它來取代在中為外掛程式\-設定的預設**QueryString** 。  
  
#### <a name="example-1"></a>範例 1
  
若要設定**QueryString**引數，以安裝識別碼 f6ce46c1 所識別的特定更新 *\-971c\-43f9\-a2aa\-783df125f003*：  
  
**QueryString\="UpdateID\=' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003 ' 和 IsInstalled\=0"**  
  
> [!NOTE]  
> 上述範例適用于使用叢集\-感知更新嚮導來套用更新。 如果您想要藉由設定自我\-更新選項與 CAU UI，或使用 [**新增\-add-cauclusterrole** ] 或 [**設定\-add-cauclusterrole**] PowerShell Cmdlet 來安裝特定更新，您必須使用兩個單一\-引號字元來格式化 UpdateID 值：  
>   
> **QueryString\="UpdateID\=' ' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003 ' ' 和 IsInstalled\=0"**  
  
#### <a name="example-2"></a>範例 2
  
設定只會安裝驅動程式的 **QueryString** 引數：  
  
**QueryString\="IsInstalled\=0，並輸入\=' 驅動程式 ' 和 IsHidden\=0"**  
  
如需**microsoft.windowsupdateplugin**中預設外掛程式\-查詢字串的詳細資訊，請參閱 \(的搜尋準則，例如**IsInstalled**\)，以及您可以在查詢字串中包含的語法，請參閱[Windows Update 代理程式（WUA） API 參考](https://go.microsoft.com/fwlink/p/?LinkId=223304)中的搜尋準則一節。  
  
## <a name="BKMK_HFP"></a>使用 Microsoft.hotfixplugin  
**Microsoft.hotfixplugin**中的「外掛程式」\-可用來套用「microsoft 有限發佈版本」 \(LDR\) 更新 \(也稱為「修復程式」，先前稱為「qfe\)」，您可以獨立下載以解決特定的 Microsoft 軟體問題。 外掛程式會從 SMB 檔案共用上的根資料夾安裝更新，也可以自訂以套用非\-的 Microsoft 驅動程式、固件和 BIOS 更新。

> [!NOTE]
> 您有時可以在知識庫檔中從 Microsoft 下載修補程式，但也會依需求將它們提供給\-客戶。

### <a name="requirements"></a>需求

- 容錯移轉叢集與遠端更新協調器電腦 \(如果使用\) 必須符合 CAU 的需求，以及在[cau 的需求和最佳作法](cluster-aware-updating-requirements.md)中所列的遠端系統管理所需的設定。
- 檢閱[使用 Microsoft.HotfixPlugin 的建議](cluster-aware-updating-requirements.md#BKMK_BP_HF)。
- 為獲得最佳結果，建議您執行 CAU 最佳做法分析程式 \(BPA\) 模型，以確保叢集和更新環境已正確設定，以使用 CAU 來套用更新。 如需詳細資訊，請參閱 [Test CAU updating readiness](cluster-aware-updating-requirements.md#BKMK_BPA)。
- 從發行者取得更新，並將它們複製或解壓縮到伺服器訊息區 \(SMB\) 檔案共用\) \(可支援至少 SMB 2.0 且可供所有叢集節點和遠端更新協調器電腦使用的叢集（如果 CAU 在遠端 \(更新模式\-）。 如需詳細資訊，請參閱本主題稍後的 [設定 Hotfix 根資料夾結構](#BKMK_HF_ROOT) 。 

    > [!NOTE]
    > 根據預設，此外掛程式\-只會安裝具有下列副檔名的修補程式： .msu、.msi 和 .msp。

- 將 **% systemroot%\\System32\\WindowsPowerShell\\v1.0\\模組**中所提供的 \(defaulthotfixconfig.xml，複製到已安裝 CAU 工具的電腦上\\的 clusterawareupdating.DLL 檔案夾，\) 至您所建立的修補程式根資料夾，並在其中解壓縮修補程式。 例如，將配置檔案複製到 *\\\\MyFileServer\\的修補程式\\根\\* 。 

    > [!NOTE]
    > 若要安裝 Microsoft 提供的大部分 Hotfix 與其他更新，您可以使用預設的 Hotfix 設定檔而無需修改。 若您的案例需要，您可以以進階工作方式自訂設定檔。 設定檔可以包含自訂規則，例如處理具有特定副檔名的 Hotfix 或定義特定結束條件行為的規則。 如需詳細資訊，請參閱本主題稍後的 [自訂 Hotfix 設定檔](#BKMK_CONFIG_FILE) 。

### <a name="configuration"></a>設定

設定下列設定。 如需詳細資訊，請參閱此主題稍後的小節的連結。
- 包含要套用之更新與 Hotfix 設定檔之共用 Hotfix 根資料夾的路徑。 您可以在 CAU UI 中輸入此路徑，或在引數中設定**HotfixRootFolderPath\=\<路徑 >** PowerShell 插入\-。 

   > [!NOTE]
   > 您可以將 [修補程式] 根資料夾指定為本機資料夾路徑或格式為 *\\\\ServerName\\共用\\RootFolderName*的 UNC 路徑。 可以使用網域\-型或獨立 DFS 命名空間路徑。 不過，在檢查修補程式設定檔中存取權限的功能中，插入\-與 DFS 命名空間路徑不相容，因此，如果您設定一個，則必須使用 CAU UI 或在引數中設定**DisableAclChecks\=' True '** \-外掛程式，以停用存取權限檢查。
- 伺服器上的設定，其裝載了修補程式根資料夾，以檢查是否有適當的許可權可存取資料夾，並確保從 SMB 共用資料夾存取之資料的完整性 \(SMB 簽署或 SMB 加密\)。 如需詳細資訊，請參閱 [限制對 Hotfix 根資料夾的存取](#BKMK_ACL)。

### <a name="additional-options"></a>其他選項

- （選擇性）在中設定插頭\-，以便在從「修補程式」檔案共用存取資料時強制執行 SMB 加密。 在 CAU UI 的 [**其他選項**] 頁面上，選取 [**存取修補程式根資料夾時需要 SMB 加密**] 選項，或在引數中設定**RequireSMBEncryption\=' True '** PowerShell 外掛程式\-。 
  > [!IMPORTANT]
  > 您必須在 SMB 伺服器上執行額外的設定步驟，以使用 SMB 簽署或 SMB 加密來啟用 SMB 資料完整性。 如需詳細資訊，請參閱 [限制對 Hotfix 根資料夾的存取](#BKMK_ACL)中的步驟 4。 若選取強制使用 SMB 加密的選項，且未使用 SMB 加密來設定對 Hotfix 根資料夾的存取權，「更新執行」將失敗。
- 或者，您可以停用是否有足夠權限可存取 Hotfix 根資料夾與 Hotfix 設定檔的預設檢查。 在 CAU UI 中，選取 [停用] [**檢查] 以取得對此修補程式根資料夾和設定檔的系統管理員存取權**，或設定**DisableAclChecks\=' True '** 外掛程式\-in 引數。
- （選擇性）設定**HotfixInstallerTimeoutMinutes\=<Integer>** 引數，以指定在等候「修補程式」的插入\-等待的時間長度。 \(預設值為30分鐘。\) 例如，若要指定兩個小時的超時期間，請將**HotfixInstallerTimeoutMinutes\=120**。
- （選擇性）設定**HotfixConfigFileName \= <name>** 在引數中插入\-，以指定位於「修補程式」根資料夾中的「修補程式」設定檔名稱。 若未指定，會使用預設名稱 DefaultHotfixConfig.xml。
  
### <a name="BKMK_HF_ROOT"></a>設定修補程式根資料夾結構

若要讓「修復外掛程式」\-能夠運作，必須在 SMB 檔案共用中，將修補程式\-定義的結構儲存在 \(的修補根資料夾\)中，而且您必須使用 CAU UI 或 CAU PowerShell Cmdlet，在中設定此「修復程式」外掛程式\-中的「修補程式」根資料夾的路徑。 這個路徑會以**HotfixRootFolderPath**引數傳遞至中的插入\-。 您可以根據您的更新需求為 Hotfix 根資料夾選擇數種結構的其中之一，如下列範例所示。 未遵守該結構的檔案或資料夾會被忽略。  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>範例 1-用來將修補程式套用至所有叢集節點的資料夾結構
  
若要指定將修補程式套用至所有叢集節點，請將它們複製到名為 CAUHotfix 的資料夾中， **\_全部都**放在此待測程式根資料夾 在此範例中， **HotfixRootFolderPath**外掛程式\-in 引數會設定為 *\\\\MyFileServer\\的修補程式\\根\\* 。 **CAUHotfix\_All**資料夾包含三個具有副檔名 .msu、.msi 和 .msp 的更新，將會套用至所有叢集節點。 更新檔案名稱只適用於示範用途。  
  
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
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>範例 2-僅用來將特定更新套用到特定節點的資料夾結構
  
若要指定只將 Hotfix 套用到特定節點，請使用 Hotfix 根資料夾下具有節點名稱的子資料夾。 使用叢集節點的 NetBIOS 名稱，例如 *ContosoNode1*。 接著，將只要套用到此節點的更新移動到此子資料夾中。 在下列範例中， **HotfixRootFolderPath**外掛程式\-in 引數會設定為 *\\\\MyFileServer\\的修補程式\\根\\* 。 CAUHotfix 中的更新 **\_所有**資料夾都會套用至所有叢集節點，而*節點 1\_特定的\_更新。 msu*只會套用至*ContosoNode1*。  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>範例 3-用來套用 .msu、.msi 和 .msp 檔案以外之更新的資料夾結構
  
根據預設值， **Microsoft.HotfixPlugin** 只會套用具有 .msu、.msi 或 .msp 副檔名的更新。 不過，特定更新可能會有不同的副檔名，而且需要不同的安裝命令。 例如，您可能需要將具有副檔名 .exe 的韌體更新套用到叢集中的節點。 您可以使用子資料夾設定 [修補程式] 根資料夾，以指出應該安裝特定、非\-預設的更新類型。 您也必須設定對應的資料夾安裝規則，以在 Hotfix 設定 XML 檔案的 `<FolderRules>` 元素中指定安裝命令。  
  
在下列範例中， **HotfixRootFolderPath**外掛程式\-in 引數會設定為 *\\\\MyFileServer\\的修補程式\\根\\* 。 有數個更新將套用到所有叢集節點，有一個韌體更新 *SpecialHotfix1.exe* 將套用到 *ContosoNode1* (使用 *FolderRule1*)。 如需有關在 Hotfix 設定檔中設定 *FolderRule1* 的資訊，請參閱此主題稍後的 [自訂 Hotfix 設定檔](#BKMK_CONFIG_FILE) 。  
  
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

### <a name="BKMK_CONFIG_FILE"></a>自訂修補程式設定檔  
Hotfix 設定檔控制 **Microsoft.HotfixPlugin** 如何在容錯移轉叢集中安裝特定的 Hotfix 檔案類型。 設定檔的 XML 結構描述是在 HotfixConfigSchema.xsd 中定義，此 .xsd 檔案位於已安裝 CAU 工具之電腦的下列資料夾：  
  
**% systemroot%\\System32\\WindowsPowerShell\\v1.0\\模組\\Clusterawareupdating.DLL 檔案夾**  
  
若要自訂 Hotfix 設定檔，請將範例設定檔 DefaultHotfixConfig.xml 從此位置複製到 Hotfix 根資料夾，並根據您的案例進行適當的修改。  
  
> [!IMPORTANT]  
> 若要套用 Microsoft 提供的大部分 Hotfix 與其他更新，您可以使用預設的 Hotfix 設定檔而無需修改。 Hotfix 設定檔自訂是進階使用案例中的工作。  
  
根據預設值，Hotfix 設定 XML 檔案會定義下列兩種 Hotfix 類別的安裝規則與結束條件：  
  
-   具有外掛程式\-的擴充功能，預設可\)\(.msu、.msi 和 .msp 檔案安裝。  
  
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
  
-   不是 .msi、.msu 或 .msp 檔案的修補程式或其他更新檔案（例如，非\-的 Microsoft 驅動程式、固件和 BIOS 更新）。  
  
    每個非\-的預設檔案類型都會設定為 `<FolderRules>` 元素中的 `<Folder>` 元素。 `<Folder>` 元素的名稱屬性必須與將包含對應類型更新之 Hotfix 根資料夾中的資料夾名稱相同。 資料夾可以在**CAUHotfix\_所有**資料夾或節點\-特定資料夾中。 例如，若已在 Hotfix 根資料夾中設定 *FolderRule1* ，請在 XML 檔案中設定下列元素，以定義該資料夾中之更新的安裝範本與結束條件。  
  
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
  
|`<Template>` 屬性|說明|  
|--------------------------|---------------|  
|`path`|`<Extension name>` 屬性中定義之檔案類型的安裝程式完整路徑。<br /><br />若要指定 Hotfix 根資料夾結構中之更新的路徑，請使用 `$update$`。|  
|`parameters`|`path`中指定之程式的必要或選擇性參數字串。<br /><br />若要指定 Hotfix 根資料夾結構中之更新檔案的路徑參數，請使用 `$update$`。|  
  
|`<ExitConditions>` 子元素|說明|  
|---------------------------------|---------------|  
|`<Success>`|定義一或多個指出所指定更新已順利完成的結束代碼。 這是必要的子元素。|  
|`<Success_RebootRequired>`|您可以定義一或多個結束代碼，指出所指定更新成功且節點必須重新啟動 <br>**注意：** （選擇性） `<Folder>` 元素可以包含 `alwaysReboot` 屬性。 若已設定此屬性，它會指出此規則安裝的 Hotfix 是否傳回 `<Success>`中定義的其中一個結束代碼，它是解譯為 `<Success_RebootRequired>` 結束條件。|  
|`<Fail_RebootRequired>`|您可以定義一或多個結束代碼，指出所指定更新失敗且節點必須重新啟動|  
|`<AlreadyInstalled>`|您可以定義一或多個結束代碼，指出所指定更新因為已安裝而未套用。|  
|`<NotApplicable>`|您可以定義一或多個結束代碼，指出所指定更新因為不適用於叢集節點而未套用。|  
  
> [!IMPORTANT]  
> 未在 `<ExitConditions>` 中明確定義的所有結束代碼都會被解譯為更新失敗而且節點未重新啟動。  
  
### <a name="BKMK_ACL"></a>限制對此修補程式根資料夾的存取  
您必須執行數個步驟來設定 SMB 檔案伺服器與檔案共用，協助保護 Hotfix 根資料夾檔案與 Hofix 設定檔，只允許在 **Microsoft.HotfixPlugin**的環境中進行存取。 這些步驟會啟用數個功能，它們可以協助防止 Hotfix 檔案遭竄改而使得容錯移轉叢集有安全之虞。  
  
一般步驟如下所示：  
  
1.  識別用來更新執行的使用者帳戶，方法是使用中的外掛程式\-  
  
2.  在 SMB 檔案伺服器上的適當群組中設定此使用者帳戶  
  
3.  設定對 Hotfix 根資料夾的存取權限  
  
4.  設定 SMB 資料完整性設定  
  
5.  在 SMB 伺服器上啟用 Windows 防火牆規則  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>步驟 1。 識別用來更新執行的使用者帳戶，方法是使用中的 [修復外掛程式]\-
  
使用**microsoft.hotfixplugin**執行「更新執行」時，在 cau 中用來檢查安全性設定的帳戶，取決於是否在遠端\-更新模式或自我\-更新模式中使用 cau，如下所示：  
  
-   **遠端\-更新模式**在叢集上具有系統管理許可權的帳戶，以預覽和套用更新。  
  
-   **自我\-更新模式**在 CAU 叢集角色的 Active Directory 中設定的虛擬電腦物件名稱。 這是針對 CAU 叢集角色在 Active Directory 中預先設置之虛擬電腦物件的名稱，或 CAU 針對叢集角色產生的名稱。 若要取得由 CAU 產生的名稱，請執行**Get\-Add-cauclusterrole** CAU PowerShell Cmdlet。 在輸出中， **ResourceGroupName** 是產生之虛擬電腦物件帳戶的名稱。  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>步驟 2。 在 SMB 檔案伺服器上的適當群組中設定此使用者帳戶
  
> [!IMPORTANT]  
> 您必須在 SMB 伺服器上新增一個帳戶，供「更新執行」做為本機系統管理員帳戶使用。 若因為您組織的安全性原則而不允許您這樣做，請使用下列程序在 SMB 伺服器上為此帳戶設定必要權限。  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>在 SMB 伺服器上設定使用者帳戶  
  
1.  新增將供「更新執行」使用的帳戶至 Distributed COM Users 群組與下列其中一個群組：Power User、Server Operation 或 Print Operator。  
  
2.  若要為該帳戶啟用必要的 WMI 權限，請啟動 SMB 伺服器上的 WMI 管理主控台。 啟動 PowerShell，然後輸入下列命令：  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  在主控台樹中，以滑鼠右鍵\-按一下 [ **WMI 控制 \(本機\)** ]，然後按一下 [內容 **]。**  
  
4.  按一下 [安全性]，然後展開 [根目錄]。  
  
5.  按一下 [CIMV2]，然後按一下 [安全性]。  
  
6.  新增將供「更新執行」使用的帳戶至 [群組或使用者名稱] 清單。  
  
7.  將「執行方法」 與「遠端啟用」 權限授與將供「更新執行」使用的帳戶。  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>步驟 3. 設定對 Hotfix 根資料夾的存取權限
  
根據預設，當您嘗試套用更新時，在中的「修復外掛程式」\-會檢查 NTFS 檔案系統許可權的設定，以存取此「修補程式」根資料夾。 如果未正確設定資料夾存取權限，則在中使用 修復外掛程式\-執行的更新可能會失敗。  
  
如果您在中使用預設的「修補程式」外掛程式\-設定，請確定資料夾存取權限符合下列需求。  
  
-   Users 群組必須具有「讀取」權限。  
  
-   如果中的「外掛程式」\-將會套用 .exe 副檔名的更新，則「使用者」群組具有「執行」許可權。  
  
-   僅允許特定安全性主體 \(但不需要\) 有寫入或修改許可權。 允許的主體是本機 Administrators 群組、SYSTEM、CREATOR OWNER 與 TrustedInstaller。 其他帳戶或群組不能有 Hotfix 根資料夾的「寫入」或「修改」權限。  
  
（選擇性）您可以停用上述的 [外掛程式]\-預設執行的檢查。 您可以透過下列兩種方式的其中一種來執行此動作：  
  
-   如果您使用 CAU PowerShell Cmdlet，請在的**CauPluginArguments**參數中設定**DisableAclChecks\=' True '** 引數，以取得中的修補程式外掛程式\-。  
  
-   若您是使用 CAU UI，請在用來設定「更新執行」選項的精靈中，選取 [其他更新選項] 頁面上的 [停用 Hotfix 根資料夾和設定檔的管理員存取權檢查] 選項。  
  
不過，做為許多環境中的最佳做法，我們建議您使用預設設定來強制這些檢查。  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>步驟 4。 設定 SMB 資料完整性設定
  
若要檢查叢集節點與 SMB 檔案共用之間連線中的資料完整性，中的「修補程式」\-外掛程式會要求您針對 SMB 簽署或 SMB 加密啟用 SMB 檔案共用上的設定。 從 Windows Server 2012 開始支援 SMB 加密，它在許多環境中提供增強的安全性和更好的效能。 您可以啟用任一設定或同時啟用這兩個設定，如下所示：  
  
-   若要啟用 SMB 簽署，請參閱 Microsoft 知識庫中的 [文章 887429](https://support.microsoft.com/kb/887429) 。  
  
-   若要為 SMB 共用資料夾啟用 SMB 加密，請在 SMB 伺服器上執行下列 PowerShell Cmdlet：  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    其中，<*ShareName*> 是 SMB 共用資料夾的名稱。  
  
（選擇性）若要強制在與 SMB 伺服器的連線中使用 SMB 加密，請選取 CAU UI 中的 [**存取修補程式根資料夾時需要 SMB 加密**] 選項，或使用 cau PowerShell Cmdlet 設定**RequireSMBEncryption\=' True '** 外掛程式引數中的\-。  
  
> [!IMPORTANT]  
> 若選取強制使用 SMB 加密的選項，且未針對使用 SMB 加密的連線設定 Hotfix 根資料夾，「更新執行」將失敗。  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>步驟 5. 在 SMB 伺服器上啟用 Windows 防火牆規則
  
您必須在 SMB 檔案伺服器上的 Windows 防火牆中的\)規則中，啟用 **檔案伺服器遠端系統管理 \(smb\-** 。 在 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中，預設會啟用此功能。  
  
## <a name="see-also"></a>請參閱  
  
-   [叢集感知更新總覽](cluster-aware-updating.md)
  
-   [叢集感知更新 Windows PowerShell Cmdlet](https://docs.microsoft.com/powershell/module/clusterawareupdating)  
  
-   [叢集感知更新外掛程式參考](https://msdn.microsoft.com/library/hh418084.aspx)  
  
