---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: 叢集感知更新需求和最佳作法
ms.prod: windows-server
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: 使用叢集感知更新在執行 Windows Server 的叢集上安裝更新的需求。
ms.openlocfilehash: 501969fad2455195bca485bd8124911d6d75378e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361319"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>叢集感知更新需求和最佳作法

>適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本節說明使用叢集[感知更新](cluster-aware-updating.md)（CAU）將更新套用到執行 Windows Server 的容錯移轉叢集所需的需求和相依性。

> [!NOTE]  
> 如果您使用**microsoft.windowsupdateplugin**以外的 a $ @ no__t-0in，您可能需要單獨驗證叢集環境是否已準備好套用更新。 如果您使用非 @ no__t-0Microsoft 外掛程式 @ no__t-1in，請洽詢發行者以取得詳細資訊。 如需外掛程式 @ no__t-0ins 的詳細資訊，請參閱「[外掛程式 @ no__t 2Ins 如何」工作](cluster-aware-updating-plug-ins.md)。   

## <a name="BKMK_REQ_CLUS"></a>安裝容錯移轉叢集功能和容錯移轉叢集工具  
CAU 要求您必須安裝「容錯移轉叢集」功能與「容錯移轉叢集工具」。 容錯移轉叢集工具組含 CAU 工具 \(clusterawareupdating @ no__t-1、容錯移轉叢集 Cmdlet，以及 CAU 作業所需的其他元件。 如需安裝「容錯移轉叢集」功能的步驟，請參閱 [安裝容錯移轉叢集功能和工具](create-failover-cluster.md#install-the-failover-clustering-feature)。  

容錯移轉叢集工具的確切安裝需求取決於 CAU 是否協調容錯移轉叢集上的叢集角色更新 @no__t-使用自我 @ no__t-1updating 模式 @ no__t-2 或從遠端電腦0by。 CAU 的 self @ no__t-0updating 模式還需要使用 CAU 工具，在容錯移轉叢集上安裝 CAU 叢集角色。    

下表摘要說明兩種 CAU 更新模式的 CAU 功能安裝需求。  

|已安裝的元件|自我 @ no__t-0updating 模式|遠端 @ no__t-0updating 模式|  
|-----------------------|-----------------------|-------------------------|  
|容錯移轉叢集功能|在所有叢集節點上是必要的|在所有叢集節點上是必要的|  
|容錯移轉叢集工具|在所有叢集節點上是必要的|-遠端 @ no__t-0updating 電腦上的必要元件<br />-必須在所有叢集節點上執行[save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)指令|  
|CAU 叢集角色|必要項|非必要|  

## <a name="obtain-an-administrator-account"></a>取得系統管理員帳戶  
下列系統管理員需求是使用 CAU 功能的必要條件。  

-   若要使用 CAU 使用者介面 \(UI @ no__t-1 或叢集感知更新 Cmdlet 來預覽或套用更新動作，您必須使用具有所有叢集節點之本機系統管理員許可權和許可權的網域帳戶。 如果帳戶在每個節點上沒有足夠的許可權，當您執行這些動作時，系統會提示您在 [叢集感知更新] 視窗中提供必要的認證。 若要使用叢集[感知更新](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)Cmdlet，您可以提供所需的認證做為 Cmdlet 參數。  

-   如果您使用不具有叢集節點之本機系統管理員許可權的帳戶登入時，在遠端 @ no__t-0updating 模式中使用 CAU，您必須在更新上使用本機系統管理員帳戶以系統管理員身分執行 CAU 工具。協調器電腦，或使用具有「在**驗證後模擬用戶端**」使用者權限的帳戶。 

-   若要執行 CAU 最佳做法分析程式，您必須使用在叢集節點上具有系統管理許可權的帳戶，以及用來執行[test-causetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) Cmdlet 或分析叢集更新之電腦上的本機管理許可權。使用叢集感知更新視窗的準備就緒。 如需詳細資訊，請參閱 [測試叢集更新整備](#BKMK_BPA)。  

## <a name="verify-the-cluster-configuration"></a>驗證叢集設定  
下列是讓容錯移轉叢集可支援使用 CAU 來更新的一般需求。 在節點上啟用遠端管理功能的額外設定需求列於此主題稍後的 [設定節點以進行遠端管理](#BKMK_NODE_CONFIG) 。  

-   足夠的叢集節點必須在線上，叢集才有仲裁。  

-   所有叢集節點都必須位於相同的 Active Directory 網域。  

-   叢集名稱必須可在網路上使用 DNS 來解析。  

-   如果在遠端 @ no__t-0updating 模式中使用 CAU，更新協調器電腦必須具有容錯移轉叢集節點的網路連線，而且必須與容錯移轉叢集位於相同的 Active Directory 網域。  

-   所有叢集節點都必須執行 Cluster 服務。 根據預設值，所有叢集節點上都會安裝此服務，而且此服務會被設定為自動啟動。  

-   若要在 CAU 更新執行期間使用 PowerShell pre @ no__t-0update 或 post @ no__t-1update 腳本，請確定腳本已安裝在所有叢集節點上，或可供所有節點存取，例如，在高可用性的網路檔案共用上。 若指令碼是儲存到網路檔案共用，請設定該資料夾，讓 Everyone 群組擁有該資料夾的「讀取」權限。  

## <a name="BKMK_NODE_CONFIG"></a>設定節點以進行遠端系統管理  
若要使用叢集感知更新，叢集的所有節點都必須設定為進行遠端系統管理。 根據預設，設定節點以進行遠端系統管理時，唯一必須執行的工作是[啟用防火牆規則以允許自動重新開機](#BKMK_FW)。 

下表列出完整的遠端系統管理需求，以防您的環境從預設值分歧。

[安裝容錯移轉叢集功能與容錯移轉叢集工具](#BKMK_REQ_CLUS)的安裝需求與此主題先前各節所述的一般叢集需求並未涵蓋這些需求。  

|需求|預設狀態|自我 @ no__t-0updating 模式|遠端 @ no__t-0updating 模式|  
|---------------|---|-----------------------|-------------------------|  
|[啟用防火牆規則以允許自動重新開機](#BKMK_FW)|已停用|若使用防火牆，在所有叢集節點上是必要的|若使用防火牆，在所有叢集節點上是必要的|  
|[啟用 Windows Management Instrumentation](#BKMK_WMI)|Enabled|在所有叢集節點上是必要的|在所有叢集節點上是必要的|  
|[啟用 Windows PowerShell 3.0 或4.0 與 Windows PowerShell 遠端執行功能](#BKMK_PS)|Enabled|在所有叢集節點上是必要的|在執行下列項目的所有叢集節點上是必要的：<br /><br />- [Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) Cmdlet<br />-PowerShell pre @ no__t-0update 和 post @ no__t-在「更新執行期間1update 腳本<br />-使用叢集感知更新視窗或[Test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) Windows PowerShell Cmdlet 來測試叢集更新準備就緒|  
|[安裝 .NET Framework 4.6 或4。5](#BKMK_NET)|Enabled|在所有叢集節點上是必要的|在執行下列項目的所有叢集節點上是必要的：<br /><br />- [Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) Cmdlet<br />-PowerShell pre @ no__t-0update 和 post @ no__t-在「更新執行期間1update 腳本<br />-使用叢集感知更新視窗或[Test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) Windows PowerShell Cmdlet 來測試叢集更新準備就緒|  

### <a name="BKMK_FW"></a>啟用防火牆規則以允許自動重新開機  
若要允許在套用更新後自動重新開機 \(if 安裝更新需要重新開機 @ no__t-1，如果叢集節點上使用 Windows 防火牆或非 @ no__t 2Microsoft 防火牆，則必須在允許的每個節點上啟用防火牆規則。下列流量：  

-   通訊協定：TCP  

-   方向：連入  

-   程式：wininit.exe  

-   連接埠：RPC 動態連接埠  

-   設定檔：Domain  

若叢集節點上使用的是 Windows 防火牆，您可以在每個叢集節點上啟用「遠端關機」 Windows 防火牆規則群組以執行此動作。 當您使用「叢集感知更新」視窗來套用更新並設定自我 @ no__t-0updating 選項時，會自動在每個叢集節點上啟用「**遠端關機**」 Windows 防火牆規則群組。  

> [!NOTE]  
> 當「 **Remote Shutdown** 」Windows 防火牆規則群組與針對 Windows 防火牆設定的群組原則設定衝突時，您無法將它啟用。    

執行下列 CAU Cmdlet 時，也會藉由指定 **– EnableFirewallRules**參數來啟用「**遠端關機**」防火牆規則群組：[Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps)、[Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps) 和 [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps)。  

下列 PowerShell 範例顯示在叢集節點上啟用自動重新開機的額外方法。  

```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>啟用 Windows Management Instrumentation （WMI） 
所有叢集節點都必須設定為使用 Windows Management Instrumentation \(WMI @ no__t-1 進行遠端系統管理。 此功能預設為啟用。  

若要手動啟用遠端管理，請執行下列動作：  

1.  在 [服務] 主控台中，啟動 **Windows Remote Management** 服務，並將啟動類型設定為 [自動]。  

2.  執行[set-wsmanquickconfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) Cmdlet，或從提高許可權的命令提示字元執行下列命令：  

    ```PowerShell  
    winrm quickconfig -q  
    ```  

若要支援 WMI 遠端處理，如果叢集節點上正在使用 Windows 防火牆，則必須在每個節點上啟用**Windows 遠端管理 \(HTTP @ no__t-2In @ no__t-3**的輸入防火牆規則。  根據預設值，此規則是啟用的。  

### <a name="BKMK_PS"></a>啟用 Windows PowerShell 和 Windows PowerShell 遠端執行功能  
若要啟用自我 @ no__t-0updating 模式和 remote @ no__t 1updating 模式中的特定 CAU 功能，必須安裝並啟用 PowerShell，才能在所有叢集節點上執行遠端命令。 根據預設，PowerShell 會安裝並啟用以進行遠端處理。  

若要啟用 PowerShell 遠端，請使用下列其中一種方法：  

-   執行[PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) Cmdlet。  

-   設定 Windows 遠端管理 \(WinRM @ no__t-2 的 domain @ no__t-0level 群組原則設定。  

如需有關啟用 PowerShell 遠端功能的詳細資訊，請參閱[關於遠端需求](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6)。  

### <a name="BKMK_NET"></a>安裝 .NET Framework 4.6 或4。5  
若要啟用自我 @ no__t-0updating 模式和 remote @ no__t-1updating 模式中的特定 CAU 功能，必須在所有叢集節點上安裝 .NET Framework 4.6 或 .NET Framework 4.5 （在 Windows Server 2012 R2 上）。 根據預設，會安裝 NET Framework。  

若要使用 PowerShell 安裝 .NET Framework 4.6 （或4.5）（如果尚未安裝），請使用下列命令：

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>使用叢集感知更新的最佳做法建議 

### <a name="BKMK_BP_WUA"></a>適用于套用 Microsoft 更新的建議

當您開始使用 CAU 在叢集上以預設的**microsoft.windowsupdateplugin**外掛程式來套用更新時，建議您停止使用其他方法在叢集節點上安裝來自 Microsoft 的軟體更新。  

> [!CAUTION]  
> 將 CAU 與自動更新個別節點的方法結合 \(on 固定時間排程 @ no__t-1 會導致無法預期的結果，包括服務中斷和非計畫停機。  

建議您遵循下列指導方針：  

-   為獲得最佳結果，建議您停用叢集節點上的自動更新設定 (例如透過 [控制台] 中的 [自動更新] 設定，或透過使用群組原則建立的設定)。  

    > [!CAUTION]  
    > 在叢集節點上自動安裝更新可能會干擾 CAU 的更新安裝作業並倒置 CAU 失敗。  

    若您需要這些設定，下列「自動更新」設定與 CAU 相容，因為系統管理員可以控制更新安裝時機：  

    -   下載更新之前通知使用者的設定與安裝更新之前通知使用者的設定  

    -   自動下載更新並在安裝之前通知使用者的設定  

    不過，若 CAU 的「更新執行」正在執行時，「自動更新」正在下載更新，「更新執行」可能需要較久的時間才能完成。  

-   請勿將更新系統（例如 Windows Server Update Services \(WSUS @ no__t-1）設定為自動套用更新 \(on 固定時間排程 @ no__t-3 到叢集節點。  

-   您應該一致地將所有叢集節點設定為使用相同的更新來源，例如 WSUS 伺服器、Windows Update 或 Microsoft Update。  

-   如果您使用設定管理系統將軟體更新套用至網路上的電腦，請從所有必要或自動更新中排除叢集節點。 設定管理系統的範例包括 Microsoft System Center Configuration Manager 2007 及 Microsoft System Center Virtual Machine Manager 2008。  

-   如果內部軟體發佈伺服器 \(for 範例中，WSUS 伺服器 @ no__t-1 是用來包含及部署更新，請確定這些伺服器正確地識別叢集節點的已核准更新。  

#### <a name="BKMK_PROXY"></a>在分公司案例中套用 Microsoft updates  
若要從 Microsoft Update 或 Windows Update 下載 Microsoft 更新到特定分公司案例中的叢集節點，您可能需要在每個節點上設定本機系統帳戶的 Proxy 設定。 例如，若您的分公司叢集使用本機 Proxy 伺服器來存取 Microsoft Update 或 Windows Update 以下載更新，您可能需要執行此動作。  

如有必要，請在每個節點上設定 WinHTTP proxy 設定以指定本機 proxy 伺服器，並設定本機位址例外 \(that 是，本機位址 @ no__t-1 的略過清單。 若要這樣做，您可以在每個叢集節點上從已提升權限的命令提示字元執行下列命令：  

```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  

其中 <*ProxyServerFQDN*> 是 proxy 伺服器的完整功能變數名稱，而 <*埠*> 是通訊埠（通常是埠443）。  

例如，若要設定本機系統帳戶的 WinHTTP proxy 設定，指定 proxy 伺服器*MyProxy.CONTOSO.com*，並使用埠443和本機位址例外，請輸入下列命令：  

```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  

### <a name="BKMK_BP_HF"></a>使用 Microsoft.hotfixplugin 的建議  

-   建議您設定 Hotfix 根資料夾與 Hotfix 設定檔的權限，只將「寫入」存取權授與用來儲存這些檔案之電腦上的本機系統管理員。 這樣有助於防止未經授權的使用者竄改這些檔案，以免容錯移轉叢集功能在您套用 Hotfix 後有安全之虞。  

-   為了協助確保伺服器訊息區的資料完整性 \(SMB @ no__t-1 連線，用來存取此修補程式根資料夾，您應該在 SMB 共用資料夾中設定 SMB 加密（如果可以設定的話）。 **Microsoft.HotfixPlugin** 要求您必須設定 SMB 簽署或 SMB 加密，以協助確保 SMB 連線的資料完整性。 

    如需詳細資訊，請參閱[限制對此修補程式根資料夾和修補程式設定檔的存取](cluster-aware-updating-plug-ins.md#BKMK_ACL)。

### <a name="additional-recommendations"></a>其他建議  

-   為避免干擾可能排定在相同時間執行的 CAU「更新執行」，請勿在已排定的維護時段排定叢集名稱物件與虛擬電腦物件的密碼變更作業。  

-   您應該針對儲存在網路共用資料夾上的 @ no__t-0update 和 post @ no__t-1update 腳本設定適當的許可權，以避免未經授權的使用者對這些檔案造成潛在的篡改。  

-   若要在自我 @ no__t-0updating 模式中設定 CAU，必須在 Active Directory 中建立 CAU 叢集角色的虛擬電腦物件 \(VCO @ no__t-2。 若容錯移轉叢集有足夠的權限，CAU 可以在新增 CAU 叢集角色時自動建立此物件。 不過，因為特定組織中的安全性原則，您可能需要在 Active Directory 預先設置該物件。 如需可執行此動作的程序，請參閱 [為叢集角色預先設置帳戶的步驟](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account)。  

-   為根據 IT 組織中的類似更新需求儲存「更新執行」設定並在容錯移轉叢集中重複使用，您可以建立「更新執行設定檔」。 此外，視更新模式而定，您可以在位於可供所有遠端「更新協調器」電腦或容錯移轉叢集存取的檔案共用中儲存「更新執行設定檔」並進行管理。 如需詳細資訊，請參閱[Advanced Options 和更新 CAU 的執行設定檔](cluster-aware-updating-options.md)。  

## <a name="BKMK_BPA"></a>測試叢集更新準備就緒  
您可以 \(BPA @ no__t-1 模型執行 CAU 最佳做法分析程式，以測試容錯移轉叢集和網路環境是否符合許多需求，以讓 CAU 套用軟體更新。 許多測試都會使用預設的隨插即用 no__t-0in、 **microsoft.windowsupdateplugin**來檢查環境是否就緒，以套用 Microsoft 更新。  

> [!NOTE]  
> 您可能需要單獨驗證您的叢集環境是否已準備好使用**microsoft.windowsupdateplugin**以外的 a no__t-0in 來套用軟體更新。 如果您使用非 @ no__t-0Microsoft 外掛程式 @ no__t-1in （例如硬體製造商所提供的），請洽詢發行者以取得詳細資訊。  

您可以使用下列兩種方式來執行 BPA：  

1.  在 CAU 主控台中選取 [分析叢集更新整備] 。 在 BPA 完成準備就緒測試之後，就會顯示測試報表。 若在叢集節點上偵測到問題，報告會指出該特定問題與問題所在節點，以便您可以採取修正動作。 測試可能需要數分鐘才能完成。  

2.  執行[test-causetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) Cmdlet。 您可以在安裝 Windows PowerShell 的容錯移轉叢集模組（容錯移轉叢集工具的一部分）的本機或遠端電腦上執行 Cmdlet。 您也可以在容錯移轉叢集的節點上執行該 Cmdlet。  

> [!NOTE]  
> -   您必須使用具有叢集節點之系統管理許可權的帳戶，以及用來執行**Test @ no__t-1CauSetup** Cmdlet 的電腦上的本機系統管理許可權，或使用叢集感知來分析叢集更新準備就緒正在更新視窗。 若要使用 [叢集感知更新] 視窗執行測試，您必須使用必要的認證登入電腦。  
> -   這些測試假設用來預覽及套用軟體更新的 CAU 工具是從用來測試叢集更新整備的電腦使用相同的使用者認證執行。  

> [!IMPORTANT]  
> 強烈建議您在下列情況中測試叢集更新整備：  
>   
> -   當您第一次使用 CAU 來套用軟體更新時。  
> -   在您新增節點至叢集後，或在叢集中變更需要執行 [驗證叢集精靈] 的硬體後。  
> -   在您變更更新來源，或變更更新設定或設定 \(other，而不是 CAU @ no__t-1，可能會影響節點上的更新應用程式。  

### <a name="tests-for-cluster-updating-readiness"></a>測試叢集更新整備  
下表列出叢集更新整備測試、一些常見問題，以及解決步驟。  


|                                                      測試                                                      |                                                                                                                                               可能的問題和影響                                                                                                                                               |                                                                                                                                                                                         解決步驟                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     容錯移轉叢集必須可供使用                                     |                                                                                       無法解析容錯移轉叢集名稱，或無法存取一或多個叢集節點。 BPA 無法執行叢集整備測試。                                                                                        |                                                                   -檢查在 BPA 執行期間指定的叢集名稱是否正確。<br />-確定叢集的所有節點都在線上且正在執行。<br />-確認 [驗證設定] Wizard 可以成功在容錯移轉叢集上執行。                                                                    |
|                    必須啟用容錯移轉叢集節點以透過 WMI 進行遠端管理                    |                                                一或多個容錯移轉叢集節點未啟用遠端系統管理，方法是使用 Windows Management Instrumentation \(WMI @ no__t-1。 若未將節點設定為允許遠端管理，CAU 將無法更新叢集節點。                                                 |                                                                                                  確定已啟用所有容錯移轉叢集節點以透過 WMI 進行遠端管理。 如需詳細資訊，請參閱此主題中的[設定節點以進行遠端管理](#BKMK_NODE_CONFIG)。                                                                                                   |
|                      應該在每個容錯移轉叢集節點上啟用 PowerShell 遠端                       |                                                           PowerShell 未安裝，或未在一或多個容錯移轉叢集節點上啟用遠端功能。 CAU 無法針對自我 @ no__t-0updating 模式設定，或使用遠端 @ no__t-1updating 模式中的特定功能。                                                            |                                                                                             請確定已在所有叢集節點上安裝 PowerShell，並已啟用遠端功能。<br /><br />如需詳細資訊，請參閱此主題中的[設定節點以進行遠端管理](#BKMK_NODE_CONFIG)。                                                                                             |
|                                            容錯移轉叢集版本                                            |                                                                            容錯移轉叢集中的一或多個節點未執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。 CAU 無法更新容錯移轉叢集。                                                                             |                                                                   確認在 BPA 執行期間所指定的容錯移轉叢集正在執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。<br /><br />如需詳細資訊，請參閱本主題中的 [驗證叢集設定](#BKMK_REQ_CLUS) 。                                                                   |
| 所有容錯移轉叢集節點上都必須安裝必要的 .NET Framework 與 Windows PowerShell 版本。 |                                                                                              .NET Framework 4.6、4.5 或 Windows PowerShell 未安裝在一或多個叢集節點上。 某些 CAU 功能可能無法運作。                                                                                              |                                                                            請確定在所有叢集節點上安裝 .NET Framework 4.6 或4.5 和 Windows PowerShell （如果需要的話）。<br /><br />如需詳細資訊，請參閱此主題中的[設定節點以進行遠端管理](#BKMK_NODE_CONFIG)。                                                                             |
|                           所有叢集節點都必須執行 Cluster 服務                           |                                                                                                            一或多個節點未執行 Cluster 服務。 CAU 無法更新容錯移轉叢集。                                                                                                             |                        -確定已在叢集中的所有節點上啟動叢集服務 \(clussvc @ no__t-1，並將其設定為自動啟動。<br />-確認 [驗證設定] Wizard 可以成功在容錯移轉叢集上執行。<br /><br />如需詳細資訊，請參閱本主題中的 [驗證叢集設定](#BKMK_REQ_CLUS) 。                         |
|     所有容錯移轉叢集節點上的「自動更新」都不應該設定為自動安裝更新。     |                                           在至少一個容錯移轉叢集節點上，「自動更新」是設定為在該節點上自動安裝 Microsoft 更新。 結合 CAU 與其他更新方法會導致未規劃的停機或無法預期的結果。                                            |                                                     若已在一或多個叢集節點上為「自動更新」設定 Windows Update 功能，請確定未將「自動更新」設定為自動安裝更新。<br /><br />如需詳細資訊，請參閱 [套用 Microsoft 更新的建議](#BKMK_BP_WUA)。                                                     |
|                          所有容錯移轉叢集節點都應該使用相同的更新來源                          |                                                    一或多個容錯移轉叢集節點未設定為針對 Microsoft 更新使用與其他節點相同的更新來源。 CAU 可能無法一致地在叢集節點上套用更新。                                                    |                                                                        確定每個叢集節點都已設定為使用相同的更新來源，例如 WSUS 伺服器、Windows Update 或 Microsoft Update。<br /><br />如需詳細資訊，請參閱 [套用 Microsoft 更新的建議](#BKMK_BP_WUA)。                                                                         |
|       必須在容錯移轉叢集中的每個節點上啟用允許遠端關機的防火牆規則       |                 一或多個容錯移轉叢集節點未啟用允許遠端關機的防火牆規則，或群組原則設定防止啟用此規則。 套用需要重新啟動節點之更新的「更新執行」可能未正確完成。                  |                                                                    如果叢集節點上正在使用 Windows 防火牆或非 @ no__t 0Microsoft 防火牆，請設定允許遠端關機的防火牆規則。<br /><br />如需詳細資訊，請參閱此主題中的[啟用防火牆規則以允許自動重新啟動](#BKMK_FW)。                                                                    |
|          每個容錯移轉叢集節點上的 Proxy 伺服器設定應該設定為本機 Proxy 伺服器          |                             一或多個容錯移轉叢集節點具有不正確的 Proxy 伺服器設定。<br /><br />若正在使用本機 Proxy 伺服器，您應該正確地設定每個節點上的 Proxy 伺服器設定，讓叢集能存取 Microsoft Update 或 Windows Update。                              |                                            確定每個叢集節點上的 WinHTTP Proxy 設定已設定為本機 Proxy 伺服器 (如果需要)。 若您的環境中未使用 Proxy 伺服器，則可以忽略此警告。<br /><br />如需詳細資訊，請參閱此主題中的[在分公司案例中套用更新](#BKMK_PROXY)。                                            |
|        CAU 叢集角色應該安裝在容錯移轉叢集上，以啟用自我 @ no__t-0updating 模式        |                                                                                                   此容錯移轉叢集上未安裝 CAU 叢集角色。 叢集自我 @ no__t-0updating 需要此角色。                                                                                                   |      若要在自我 @ no__t-0updating 模式中使用 CAU，請以下列其中一種方式在容錯移轉叢集中新增 CAU 叢集角色：<br /><br />-執行[Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) PowerShell Cmdlet。<br />-在 [叢集感知更新] 視窗中選取 [**設定叢集自我 @ no__t-1updating 選項**] 動作。      |
|         應該在容錯移轉叢集上啟用 CAU 叢集角色，以啟用自我 @ no__t-0updating 模式         | CAU 叢集角色已停用。 例如，未安裝 CAU 叢集角色，或已使用[Disable @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) PowerShell Cmdlet 予以停用。 叢集自我 @ no__t-0updating 需要此角色。 | 若要在自我 @ no__t-0updating 模式中使用 CAU，請以下列其中一種方式在此容錯移轉叢集上啟用 CAU 叢集角色：<br /><br />-執行[Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) PowerShell Cmdlet。<br />-在 [叢集感知更新] 視窗中選取 [**設定叢集自我 @ no__t-1updating 選項**] 動作。 |
|      針對自我 @ no__t-1updating 模式設定的 CAU 外掛程式 @ no__t-0in，必須在所有容錯移轉叢集節點上註冊      |                                                              此容錯移轉叢集的一或多個節點上的 CAU 叢集角色無法存取在 self @ no__t-1updating 選項中設定的 CAU 外掛程式 @ no__t-0in 模組。 自我 @ no__t 0updating 執行可能會失敗。                                                              |           -請遵循提供 CAU 外掛程式 @ no__t-1in 之產品的安裝程式，確認已在所有叢集節點上安裝設定的 CAU 外掛程式 @ no__t 0in。<br />-執行[register @ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) PowerShell Cmdlet，在所需的叢集節點上註冊外掛程式 @ no__t-2in。           |
|                所有容錯移轉叢集節點都應該具有一組相同的已註冊 CAU 外掛程式 @ no__t-0ins                 |                                                                             如果在「更新執行」中設定要使用的「外掛程式 @ no__t 1in」已變更為所有叢集節點上都無法使用的，則自我 @ no__t 0updating 執行可能會失敗。                                                                              |           -請遵循提供 CAU 外掛程式 @ no__t-1in 之產品的安裝程式，確認已在所有叢集節點上安裝設定的 CAU 外掛程式 @ no__t 0in。<br />-執行[register @ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) PowerShell Cmdlet，在所需的叢集節點上註冊外掛程式 @ no__t-2in。           |
|                               已設定的「更新執行」選項必須是有效的                                |                                                                          為此容錯移轉叢集設定的 self @ no__t-0updating 排程和更新執行選項不完整或無效。 自我 @ no__t 0updating 執行可能會失敗。                                                                           |                                                            設定有效的自我 @ no__t-0updating 排程和一組更新執行選項。 例如，您可以使用[Set @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) PowerShell Cmdlet 來設定 CAU 叢集角色。                                                            |
|                  至少必須有兩個容錯移轉叢集節點是 CAU 叢集角色的擁有者                  |                                                                                        在 [自我 @ no__t-0updating] 模式中啟動的「更新執行」將會失敗，因為 CAU 叢集角色沒有可移至的可能擁有者節點。                                                                                         |                                                                                                                使用「容錯移轉叢集工具」來確定所有叢集節點都已設定為 CAU 叢集角色的可能擁有者。 這是預設設定。                                                                                                                |
|                  所有容錯移轉叢集節點都必須能存取 Windows PowerShell 指令碼                  |                                                                        並非 CAU 叢集角色的所有可能擁有者節點都可以存取已設定的 Windows PowerShell pre @ no__t-0update 和 post @ no__t-1update 腳本。 自我 @ no__t 0updating 執行將會失敗。                                                                        |                                                                                                                    確定 CAU 叢集角色的所有可能擁有者節點都有權存取已設定的 PowerShell 前置 @ no__t-0update 和 post @ no__t-1update 腳本。                                                                                                                     |
|                   所有容錯移轉叢集節點都應該使用相同的 Windows PowerShell 指令碼                   |                                                     並非 CAU 叢集角色的所有可能擁有者節點都使用指定的 Windows PowerShell pre @ no__t-0update 和 post @ no__t-1update 腳本的相同複本。 自我 @ no__t 0updating 執行可能會失敗或顯示未預期的行為。                                                     |                                                                                                                                   確定 CAU 叢集角色的所有可能擁有者節點都使用相同的 PowerShell 前置 @ no__t-0update 和 post @ no__t-1update 腳本。                                                                                                                                   |
|         為「更新執行」指定的 WarnAfter 設定應該小於 StopAfter 設定         |                                                                           指定的 CAU「更新執行」逾時值使得警告逾時無效。 「更新執行」可能會在警告事件記錄檔產生之前就被取消。                                                                            |                                                                                                                                      在「更新執行」選項中，為 **WarnAfter** 選項值設定小於 **StopAfter** 選項值的值。                                                                                                                                       |

## <a name="see-also"></a>另請參閱  

-   [叢集感知更新總覽](cluster-aware-updating.md)