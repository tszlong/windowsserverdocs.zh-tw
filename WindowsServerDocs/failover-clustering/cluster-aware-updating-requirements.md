---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: 叢集感知更新需求和最佳做法
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: 使用在執行 Windows Server 的叢集上安裝更新的 叢集感知更新需求。
ms.openlocfilehash: 379c3caa39b09e8a912150f2423190e143991c05
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819729"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>叢集感知更新需求和最佳做法

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本章節描述的需求和使用所需的相依性[Cluster-aware Updating](cluster-aware-updating.md) (CAU) 將更新套用至執行 Windows Server 容錯移轉叢集。
  
> [!NOTE]  
> 您可能需要另行驗證您的叢集環境是否準備好套用更新，如果您使用隨\-中而非**Microsoft.WindowsUpdatePlugin**。 如果您使用非\-Microsoft 隨插即用\-中，請連絡發行者如需詳細資訊。 如需有關隨\-集，請參閱 <<c2> [ 如何插入\-集運作](cluster-aware-updating-plug-ins.md)。   
  
## <a name="BKMK_REQ_CLUS"></a>安裝容錯移轉叢集功能與容錯移轉叢集工具  
CAU 要求您必須安裝「容錯移轉叢集」功能與「容錯移轉叢集工具」。 容錯移轉叢集工具包含 CAU 工具\(clusterawareupdating.dll\)，容錯移轉叢集 cmdlet 與 CAU 操作所需的其他元件。 如需安裝「容錯移轉叢集」功能的步驟，請參閱 [安裝容錯移轉叢集功能和工具](create-failover-cluster.md#install-the-failover-clustering-feature)。  
  
容錯移轉叢集工具的確切安裝需求視 CAU 協調更新為叢集角色容錯移轉叢集上的是否\(使用 self\-更新模式\)或從遠端電腦。 自我\-此外更新的 CAU 的模式使用 CAU 工具需要安裝 CAU 叢集角色容錯移轉叢集上。    
  
下表摘要說明兩種 CAU 更新模式的 CAU 功能安裝需求。  
  
|已安裝的元件|自助\-更新模式|遠端\-更新模式|  
|-----------------------|-----------------------|-------------------------|  
|容錯移轉叢集功能|在所有叢集節點上是必要的|在所有叢集節點上是必要的|  
|容錯移轉叢集工具|在所有叢集節點上是必要的|-需要遠端\-更新的電腦<br />-才能在所有叢集節點上執行[Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet|  
|CAU 叢集角色|必要項|非必要|  
  
## <a name="obtain-an-administrator-account"></a>取得系統管理員帳戶  
下列系統管理員需求是使用 CAU 功能的必要條件。  
  
-   若要預覽或套用更新使用 CAU 使用者介面\(UI\)或叢集感知更新 cmdlet，您必須使用所有叢集節點擁有本機系統管理員權限的網域帳戶。 如果帳戶沒有足夠的權限的每個節點，會提示您提供所需的認證，當您執行這些動作在叢集感知更新 視窗中。 若要使用[Cluster-aware Updating](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) cmdlet，您可以在 cmdlet 參數的形式提供必要的認證。  
  
-   如果您在遠端使用 CAU\-更新模式，當您登入不在叢集節點具有本機系統管理員權限和權限的帳戶，您必須執行 CAU 工具以系統管理員身分使用上的本機系統管理員帳戶更新協調器的電腦，或使用具有的帳戶**驗證後模擬用戶端**使用者權限。 
  
-   若要執行 「 CAU 最佳做法分析程式，您必須使用可在叢集節點上的系統管理權限來執行的電腦上的本機系統管理權限的帳戶[Test-causetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet，或分析叢集更新整備使用叢集感知更新的視窗。 如需詳細資訊，請參閱 [測試叢集更新整備](#BKMK_BPA)。  
  
## <a name="verify-the-cluster-configuration"></a>驗證叢集設定  
下列是讓容錯移轉叢集可支援使用 CAU 來更新的一般需求。 在節點上啟用遠端管理功能的額外設定需求列於此主題稍後的 [設定節點以進行遠端管理](#BKMK_NODE_CONFIG) 。  
  
-   足夠的叢集節點必須在線上，叢集才有仲裁。  
  
-   所有叢集節點都必須位於相同的 Active Directory 網域。  
  
-   叢集名稱必須可在網路上使用 DNS 來解析。  
  
-   如果在遠端使用 CAU\-更新模式，更新協調器 」 電腦必須有網路連線到容錯移轉叢集節點中，以及它必須位於相同的 Active Directory 網域和容錯移轉叢集。  
  
-   所有叢集節點都必須執行 Cluster 服務。 根據預設值，所有叢集節點上都會安裝此服務，而且此服務會被設定為自動啟動。  
  
-   若要使用 PowerShell pre\-更新或張貼\-CAU 更新執行 」 期間更新指令碼，請確定指令碼會安裝在所有叢集節點上，或，都能夠存取所有節點，例如，高可用性網路檔案共用上。 若指令碼是儲存到網路檔案共用，請設定該資料夾，讓 Everyone 群組擁有該資料夾的「讀取」權限。  
  
## <a name="BKMK_NODE_CONFIG"></a>設定節點以進行遠端管理  
若要使用叢集感知更新，叢集的所有節點都必須都設定遠端管理。 根據預設，唯一的工作，必須執行遠端管理是要設定節點[啟用防火牆規則以允許自動重新啟動](#BKMK_FW)。 

下表列出的完整的遠端管理需求，萬一您的環境分歧時的預設值。

[安裝容錯移轉叢集功能與容錯移轉叢集工具](#BKMK_REQ_CLUS)的安裝需求與此主題先前各節所述的一般叢集需求並未涵蓋這些需求。  
  
|需求|預設狀態|自助\-更新模式|遠端\-更新模式|  
|---------------|---|-----------------------|-------------------------|  
|[啟用防火牆規則以允許自動重新啟動](#BKMK_FW)|已停用|若使用防火牆，在所有叢集節點上是必要的|若使用防火牆，在所有叢集節點上是必要的|  
|[啟用 Windows Management Instrumentation](#BKMK_WMI)|Enabled|在所有叢集節點上是必要的|在所有叢集節點上是必要的|  
|[啟用 Windows PowerShell 3.0 或 4.0 和 Windows PowerShell 遠端執行功能](#BKMK_PS)|Enabled|在所有叢集節點上是必要的|在執行下列項目的所有叢集節點上是必要的：<br /><br />- [Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-PowerShell pre\-更新，並張貼\-更新執行 」 期間更新指令碼<br />-測試叢集更新整備程度使用叢集感知更新的視窗或[測試\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) Windows PowerShell cmdlet|  
|[安裝.NET Framework 4.6 或 4.5](#BKMK_NET)|Enabled|在所有叢集節點上是必要的|在執行下列項目的所有叢集節點上是必要的：<br /><br />- [Save-caudebugtrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-PowerShell pre\-更新，並張貼\-更新執行 」 期間更新指令碼<br />-測試叢集更新整備程度使用叢集感知更新的視窗或[測試\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) Windows PowerShell cmdlet|  

### <a name="BKMK_FW"></a>啟用防火牆規則以允許自動重新啟動  
若要允許自動重新啟動，套用更新後\(如果安裝更新需要重新啟動\)，如果 Windows 防火牆或非\-Microsoft 防火牆正在叢集節點上的使用中，必須在啟用防火牆規則每個節點，以允許下列流量：  
  
-   通訊協定：TCP  
  
-   方向：連入  
  
-   程式：wininit.exe  
  
-   連接埠：RPC 動態連接埠  
  
-   設定檔：網域  
  
若叢集節點上使用的是 Windows 防火牆，您可以在每個叢集節點上啟用「遠端關機」  Windows 防火牆規則群組以執行此動作。 當您使用叢集感知更新 視窗來套用更新，並設定自我\-更新選項 **Remote Shutdown** Windows 防火牆規則群組會自動啟用每個叢集節點上。  
  
> [!NOTE]  
> 當「 **Remote Shutdown** 」Windows 防火牆規則群組與針對 Windows 防火牆設定的群組原則設定衝突時，您無法將它啟用。    
  
**遠端關機**防火牆規則群組也會啟用指定 **– EnableFirewallRules**執行下列 CAU cmdlet 時的參數：Add-CauClusterRole、[Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps) 和 [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps)。  
  
下列 PowerShell 範例會顯示啟用自動重新啟動叢集節點上的其他方法。  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>啟用 Windows Management Instrumentation (WMI) 
所有叢集節點必須都設定遠端管理使用 Windows Management Instrumentation \(WMI\)。 此功能預設為啟用。  
  
若要手動啟用遠端管理，請執行下列動作：  
  
1.  在 [服務] 主控台中，啟動 **Windows Remote Management** 服務，並將啟動類型設定為 [自動] 。  
  
2.  執行[Set-wsmanquickconfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) cmdlet 或執行下列命令從提升權限的命令提示字元：  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
若要支援 WMI 遠端執行功能，所以如果 Windows 防火牆正在叢集節點上的使用中，輸入的防火牆規則**Windows 遠端管理\(HTTP\-中\)** 必須啟用每個節點上。  根據預設值，此規則是啟用的。  
  
### <a name="BKMK_PS"></a>啟用 Windows PowerShell 和 Windows PowerShell 遠端執行功能  
若要啟用自助式\-更新模式與遠端的特定 CAU 功能\-更新模式，PowerShell 必須已安裝並啟用所有叢集節點上執行遠端命令。 根據預設，PowerShell 是安裝和啟用遠端執行功能。  
  
若要啟用 PowerShell 遠端執行功能，請使用下列方法之一：  
  
-   執行[Enable-psremoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) cmdlet。  
  
-   設定網域\-層級的群組原則設定 Windows 遠端管理\(WinRM\)。  
  
如需啟用 PowerShell 遠端執行功能的詳細資訊，請參閱[關於遠端需求](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6)。  
  
### <a name="BKMK_NET"></a>安裝.NET Framework 4.6 或 4.5  
若要啟用自助式\-更新模式與遠端的特定 CAU 功能\-必須安裝在所有叢集節點上的更新模式、.NET Framework 4.6 或.NET Framework 4.5 （在 Windows Server 2012 R2)。 根據預設，NET Framework 已安裝。  

若要安裝.NET Framework 4.6 （或 4.5） 使用 PowerShell，如果尚未安裝，請使用下列命令：

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>使用叢集感知更新的最佳做法建議 
  
### <a name="BKMK_BP_WUA"></a>套用 Microsoft 更新的建議

我們建議當您開始使用 CAU 來套用預設值的更新**Microsoft.WindowsUpdatePlugin**插入\-在叢集上，您會停止使用其他方法在叢集上，安裝來自 Microsoft 的軟體更新節點。  
  
> [!CAUTION]  
> 結合 CAU 與自動更新個別節點的方法\(在固定的時間排程\)可能造成無法預期的結果，包括服務和未規劃的停機時間的中斷。  
  
建議您遵循下列指導方針：  
  
-   為獲得最佳結果，建議您停用叢集節點上的自動更新設定 (例如透過 [控制台] 中的 [自動更新] 設定，或透過使用群組原則建立的設定)。  
  
    > [!CAUTION]  
    > 在叢集節點上自動安裝更新可能會干擾 CAU 的更新安裝作業並倒置 CAU 失敗。  
  
    若您需要這些設定，下列「自動更新」設定與 CAU 相容，因為系統管理員可以控制更新安裝時機：  
  
    -   下載更新之前通知使用者的設定與安裝更新之前通知使用者的設定  
  
    -   自動下載更新並在安裝之前通知使用者的設定  
  
    不過，若 CAU 的「更新執行」正在執行時，「自動更新」正在下載更新，「更新執行」可能需要較久的時間才能完成。  
  
-   未設定更新系統，例如 Windows Server Update Services \(WSUS\)自動套用更新\(在固定的時間排程\)到叢集節點。  
  
-   您應該一致地將所有叢集節點設定為使用相同的更新來源，例如 WSUS 伺服器、Windows Update 或 Microsoft Update。  
  
-   如果您使用設定管理系統將軟體更新套用至網路上的電腦，請從所有必要或自動更新中排除叢集節點。 設定管理系統的範例包括 Microsoft System Center Configuration Manager 2007 及 Microsoft System Center Virtual Machine Manager 2008。  
  
-   如果內部軟體散發伺服器\(比方說，WSUS 伺服器\)用來包含和部署更新，請確定這些伺服器會正確地識別已核准的更新叢集節點。  
  
#### <a name="BKMK_PROXY"></a>套用 Microsoft 更新，在分公司案例  
若要從 Microsoft Update 或 Windows Update 下載 Microsoft 更新到特定分公司案例中的叢集節點，您可能需要在每個節點上設定本機系統帳戶的 Proxy 設定。 例如，若您的分公司叢集使用本機 Proxy 伺服器來存取 Microsoft Update 或 Windows Update 以下載更新，您可能需要執行此動作。  
  
如有必要，請指定本機 proxy 伺服器，並設定本機位址例外狀況的每個節點上設定 WinHTTP proxy 設定\(亦即，本機位址略過清單\)。 若要這樣做，您可以在每個叢集節點上從已提升權限的命令提示字元執行下列命令：  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
其中 <*ProxyServerFQDN*> 是 proxy 伺服器的完整的網域名稱和 <*連接埠*> 是要進行通訊的連接埠 （通常是連接埠 443）。  
  
例如，若要設定指定 proxy 伺服器的本機系統帳戶的 WinHTTP proxy 設定*MyProxy.CONTOSO.com*、 連接埠 443 與本機位址例外，輸入下列命令：  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>使用 Microsoft.HotfixPlugin 的建議  
  
-   建議您設定 Hotfix 根資料夾與 Hotfix 設定檔的權限，只將「寫入」存取權授與用來儲存這些檔案之電腦上的本機系統管理員。 這樣有助於防止未經授權的使用者竄改這些檔案，以免容錯移轉叢集功能在您套用 Hotfix 後有安全之虞。  
  
-   為了協助確保資料完整性，以伺服器訊息區\(SMB\)用來存取 hotfix 根資料夾的連線，您應該在 SMB 共用資料夾中，設定 SMB 加密，才可以設定它。 **Microsoft.HotfixPlugin** 要求您必須設定 SMB 簽署或 SMB 加密，以協助確保 SMB 連線的資料完整性。 
  
    如需詳細資訊，請參閱 <<c0> [ 限制對 hotfix 根資料夾與 hotfix 設定檔存取](cluster-aware-updating-plug-ins.md#BKMK_ACL)。
  
### <a name="additional-recommendations"></a>其他建議  
  
-   為避免干擾可能排定在相同時間執行的 CAU「更新執行」，請勿在已排定的維護時段排定叢集名稱物件與虛擬電腦物件的密碼變更作業。  
  
-   您應該預先設定適當的權限\-更新，並張貼\-更新指令碼是儲存在網路上共用資料夾，以防止竄改這些檔案，未經授權的使用者。  
  
-   若要在自我設定 CAU\-更新模式中，虛擬電腦物件\(VCO\)必須在 Active Directory 中建立針對 CAU 叢集的角色。 若容錯移轉叢集有足夠的權限，CAU 可以在新增 CAU 叢集角色時自動建立此物件。 不過，因為特定組織中的安全性原則，您可能需要在 Active Directory 預先設置該物件。 如需可執行此動作的程序，請參閱 [為叢集角色預先設置帳戶的步驟](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account)。  
  
-   為根據 IT 組織中的類似更新需求儲存「更新執行」設定並在容錯移轉叢集中重複使用，您可以建立「更新執行設定檔」。 此外，視更新模式而定，您可以在位於可供所有遠端「更新協調器」電腦或容錯移轉叢集存取的檔案共用中儲存「更新執行設定檔」並進行管理。 如需詳細資訊，請參閱 <<c0> [ 進階選項與 cau 更新執行設定檔](cluster-aware-updating-options.md)。  
  
## <a name="BKMK_BPA"></a>測試叢集更新整備  
您可以執行 CAU 最佳做法分析程式\(BPA\)模型來測試是否在容錯移轉叢集和網路環境符合 CAU 來套用軟體更新的需求。 有許多測試都會檢查環境的使用預設外掛程式來套用 Microsoft 更新整備小幫手\-中， **Microsoft.WindowsUpdatePlugin**。  
  
> [!NOTE]  
> 您可能需要另行驗證您的叢集環境是否準備好要使用隨插即用來套用軟體更新\-中而非**Microsoft.WindowsUpdatePlugin**。 如果您使用非\-Microsoft 隨插即用\-中，例如您的硬體製造商所提供的其中一個連絡發行者如需詳細資訊。  
  
您可以使用下列兩種方式來執行 BPA：  
  
1.  在 CAU 主控台中選取 [分析叢集更新整備]  。 當 BPA 完成整備測試之後，便會出現測試報告。 若在叢集節點上偵測到問題，報告會指出該特定問題與問題所在節點，以便您可以採取修正動作。 測試可能需要數分鐘才能完成。  
  
2.  執行[Test-causetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) cmdlet。 您可以在容錯移轉叢集 Windows PowerShell 模組 （容錯移轉叢集工具的一部分） 安裝所在的本機或遠端電腦上執行 cmdlet。 您也可以在容錯移轉叢集的節點上執行該 Cmdlet。  
  
> [!NOTE]  
> -   您必須使用可在叢集節點上的系統管理權限來執行的電腦上的本機系統管理權限的帳戶**測試\-CauSetup** cmdlet 或來分析叢集更新整備使用叢集感知更新的視窗。 若要執行測試使用叢集感知更新的視窗，您必須到的電腦所需的認證進行登入。  
> -   這些測試假設用來預覽及套用軟體更新的 CAU 工具是從用來測試叢集更新整備的電腦使用相同的使用者認證執行。  
  
> [!IMPORTANT]  
> 強烈建議您在下列情況中測試叢集更新整備：  
>   
> -   當您第一次使用 CAU 來套用軟體更新時。  
> -   在您新增節點至叢集後，或在叢集中變更需要執行 [驗證叢集精靈] 的硬體後。  
> -   您變更更新來源，或變更更新設定後\(非 CAU\)可能會影響應用程式在節點上的更新。  
  
### <a name="tests-for-cluster-updating-readiness"></a>測試叢集更新整備  
下表列出叢集更新整備測試、一些常見問題，以及解決步驟。  
  
|測試|可能的問題和影響|解決步驟|  
|--------|-------------------------------|--------------------|  
|容錯移轉叢集必須可供使用|無法解析容錯移轉叢集名稱，或無法存取一或多個叢集節點。 BPA 無法執行叢集整備測試。|-檢查執行 BPA 時指定的叢集名稱的拼字。<br />-確定叢集的所有節點都已上線並正確執行。<br />-檢查，驗證設定精靈 可以順利執行容錯移轉叢集上。|  
|必須啟用容錯移轉叢集節點以透過 WMI 進行遠端管理|一或多個容錯移轉叢集節點不會啟用遠端管理使用 Windows Management Instrumentation \(WMI\)。 若未將節點設定為允許遠端管理，CAU 將無法更新叢集節點。|確定已啟用所有容錯移轉叢集節點以透過 WMI 進行遠端管理。 如需詳細資訊，請參閱此主題中的[設定節點以進行遠端管理](#BKMK_NODE_CONFIG)。|  
|應該在每個容錯移轉叢集節點上啟用 PowerShell 遠端|PowerShell 未安裝，或未啟用一或多個容錯移轉叢集節點上的遠端執行功能。 CAU 無法設定自助式\-更新模式或遠端使用某些功能\-更新模式。|請確定 PowerShell 安裝在所有叢集節點上，並已啟用遠端執行功能。<br /><br />如需詳細資訊，請參閱此主題中的[設定節點以進行遠端管理](#BKMK_NODE_CONFIG)。|  
|容錯移轉叢集版本|Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 容錯移轉叢集中的一或多個節點不會執行。 CAU 無法更新容錯移轉叢集。|確認執行 BPA 時指定的容錯移轉叢集正在執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。<br /><br />如需詳細資訊，請參閱本主題中的 [驗證叢集設定](#BKMK_REQ_CLUS) 。|  
|所有容錯移轉叢集節點上都必須安裝必要的 .NET Framework 與 Windows PowerShell 版本。|.NET framework 4.6 中，4.5 或 Windows PowerShell 未安裝在一或多個叢集節點上。 某些 CAU 功能可能無法運作。|如有必要，請確定所有的叢集節點上已安裝.NET Framework 4.6 或 4.5 和 Windows PowerShell。<br /><br />如需詳細資訊，請參閱此主題中的[設定節點以進行遠端管理](#BKMK_NODE_CONFIG)。|  
|所有叢集節點都必須執行 Cluster 服務|一或多個節點未執行 Cluster 服務。 CAU 無法更新容錯移轉叢集。|-確定叢集服務\(clussvc\)在叢集中的所有節點上啟動，且設定為自動啟動。<br />-檢查，驗證設定精靈 可以順利執行容錯移轉叢集上。<br /><br />如需詳細資訊，請參閱本主題中的 [驗證叢集設定](#BKMK_REQ_CLUS) 。|  
|所有容錯移轉叢集節點上的「自動更新」都不應該設定為自動安裝更新。|在至少一個容錯移轉叢集節點上，「自動更新」是設定為在該節點上自動安裝 Microsoft 更新。 結合 CAU 與其他更新方法會導致未規劃的停機或無法預期的結果。|若已在一或多個叢集節點上為「自動更新」設定 Windows Update 功能，請確定未將「自動更新」設定為自動安裝更新。<br /><br />如需詳細資訊，請參閱 [套用 Microsoft 更新的建議](#BKMK_BP_WUA)。|  
|所有容錯移轉叢集節點都應該使用相同的更新來源|一或多個容錯移轉叢集節點未設定為針對 Microsoft 更新使用與其他節點相同的更新來源。 CAU 可能無法一致地在叢集節點上套用更新。|確定每個叢集節點都已設定為使用相同的更新來源，例如 WSUS 伺服器、Windows Update 或 Microsoft Update。<br /><br />如需詳細資訊，請參閱 [套用 Microsoft 更新的建議](#BKMK_BP_WUA)。|  
|必須在容錯移轉叢集中的每個節點上啟用允許遠端關機的防火牆規則|一或多個容錯移轉叢集節點未啟用允許遠端關機的防火牆規則，或群組原則設定防止啟用此規則。 套用需要重新啟動節點之更新的「更新執行」可能未正確完成。|如果 Windows 防火牆或非\-Microsoft 防火牆正在叢集節點上的使用中，設定允許遠端關機的防火牆規則。<br /><br />如需詳細資訊，請參閱此主題中的[啟用防火牆規則以允許自動重新啟動](#BKMK_FW)。|  
|每個容錯移轉叢集節點上的 Proxy 伺服器設定應該設定為本機 Proxy 伺服器|一或多個容錯移轉叢集節點具有不正確的 Proxy 伺服器設定。<br /><br />若正在使用本機 Proxy 伺服器，您應該正確地設定每個節點上的 Proxy 伺服器設定，讓叢集能存取 Microsoft Update 或 Windows Update。|確定每個叢集節點上的 WinHTTP Proxy 設定已設定為本機 Proxy 伺服器 (如果需要)。 若您的環境中未使用 Proxy 伺服器，則可以忽略此警告。<br /><br />如需詳細資訊，請參閱此主題中的[在分公司案例中套用更新](#BKMK_PROXY)。|  
|CAU 叢集的角色應該安裝在容錯移轉叢集，若要啟用自助式\-更新模式|此容錯移轉叢集上未安裝 CAU 叢集角色。 此角色，才能讓叢集本身\-更新。|若要使用 CAU 中自我\-更新模式中，容錯移轉叢集上新增 CAU 叢集的角色中的下列方法之一：<br /><br />-執行[Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) PowerShell cmdlet。<br />-選取**設定叢集自行\-更新選項**叢集感知更新 視窗中的動作。|  
|CAU 叢集的角色應該容錯移轉叢集上啟用，若要啟用自助式\-更新模式|CAU 叢集角色已停用。 例如，未安裝 CAU 叢集的角色，或已停用使用[停用\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) PowerShell cmdlet。 此角色，才能讓叢集本身\-更新。|若要使用 CAU 中自我\-更新模式，其中一種以下列方式啟用此容錯移轉叢集上的 CAU 叢集的角色：<br /><br />-執行[Enable-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) PowerShell cmdlet。<br />-選取**設定叢集自行\-更新選項**叢集感知更新 視窗中的動作。|  
|已設定的 CAU 外掛程式\-中的自我\-必須註冊在所有容錯移轉叢集節點上的更新模式|此容錯移轉叢集的一或多個節點上的 CAU 叢集的角色無法存取 CAU 外掛程式\-模組中的設定中自我\-更新選項。 自我\-更新執行可能會失敗。|-確定已設定的 CAU 外掛程式\-安裝在所有叢集節點上依照提供 CAU 外掛程式之產品的安裝程序\-中。<br />-執行[註冊\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) PowerShell cmdlet，來註冊隨插即用\-中必要的叢集節點上。|  
|所有容錯移轉叢集節點都應該有相同的已註冊的 CAU 外掛程式集合\-集|自我\-更新執行可能會失敗，如果插頭\-設定中，以供 「 更新執行變更並不適用於所有叢集節點的其中一個。|-確定已設定的 CAU 外掛程式\-安裝在所有叢集節點上依照提供 CAU 外掛程式之產品的安裝程序\-中。<br />-執行[註冊\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) PowerShell cmdlet，來註冊隨插即用\-中必要的叢集節點上。|  
|已設定的「更新執行」選項必須是有效的|自我\-更新排程 」 和 「 更新執行設定為此容錯移轉叢集的選項並不完整或無效。 自我\-更新執行可能會失敗。|設定有效的自我\-更新排程並更新執行 」 選項的設定。 例如，您可以使用[設定\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) PowerShell cmdlet 來設定 CAU 叢集角色。|  
|至少必須有兩個容錯移轉叢集節點是 CAU 叢集角色的擁有者|「 更新執行中自我啟動\-更新模式將會失敗，因為 CAU 叢集的角色並沒有要移向可能的擁有者節點。|使用「容錯移轉叢集工具」來確定所有叢集節點都已設定為 CAU 叢集角色的可能擁有者。 這是預設設定。||  
|所有容錯移轉叢集節點都必須能存取 Windows PowerShell 指令碼|並非所有的可能擁有者節點，CAU 叢集角色可以存取已設定的 Windows PowerShell 預先\-更新，並張貼\-更新指令碼。 自我\-更新執行將會失敗。|確定 CAU 叢集角色的所有可能的擁有者節點都有權限可以存取已設定的 PowerShell 預先\-更新，並張貼\-更新指令碼。|  
|所有容錯移轉叢集節點都應該使用相同的 Windows PowerShell 指令碼|使用指定的 Windows PowerShell 之前的相同複本，並非所有的可能擁有者節點，CAU 叢集角色的\-更新，並張貼\-更新指令碼。 自我\-更新執行可能失敗，或出現未預期的行為。|確定 CAU 叢集角色的所有可能的擁有者節點都使用相同的 PowerShell pre\-更新，並張貼\-更新指令碼。|  
|為「更新執行」指定的 WarnAfter 設定應該小於 StopAfter 設定|指定的 CAU「更新執行」逾時值使得警告逾時無效。 「更新執行」可能會在警告事件記錄檔產生之前就被取消。|在「更新執行」選項中，為 **WarnAfter** 選項值設定小於 **StopAfter** 選項值的值。|  
  
## <a name="see-also"></a>另請參閱  
  
-   [叢集感知更新概觀](cluster-aware-updating.md)