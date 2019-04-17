---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: "叢集更新需求與最佳做法"
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 4/28/2017
description: "使用叢集更新安裝的更新，在執行 Windows Server 叢集需求。"
ms.openlocfilehash: 8517466d58345077af446c1b2c1e2aeb3b17aaa1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>叢集更新需求與最佳做法

>適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本節需求和相依性會需要使用[更新叢集](cluster-aware-updating.md)(CAU) 上執行 Windows Server 容錯移轉叢集套用更新。
  
> [!NOTE]  
> 您可能需要獨立驗證叢集環境可套用更新，如果您不使用 plug\ 在**Microsoft.WindowsUpdatePlugin**。 如果您使用 non\ Microsoft plug\ 中，與發行者連絡的詳細資訊。 如需 plug\ 集的詳細資訊，請查看[Plug\ 集的運作方式](cluster-aware-updating-plug-ins.md)。   
  
## <a name="BKMK_REQ_CLUS"></a>安裝容錯功能與容錯移轉叢集工具  
CAU 需要安裝容錯功能和容錯移轉叢集工具。 容錯移轉叢集工具包含 CAU 工具 \(clusterawareupdating.dll\)、容錯 cmdlet 和 CAU 作業需要其他元件。 步驟以安裝容錯功能，請查看[安裝容錯移轉叢集功能與工具](http://go.microsoft.com/fwlink/p/?LinkId=253342)。  
  
CAU 是否為容錯移轉叢集上叢集角色座標更新而定容錯移轉叢集工具的確切安裝需求 \（透過 self\ 更新 mode\）或從遠端電腦。 CAU self\ 更新模式此外需要安裝 CAU 叢集角色容錯移轉叢集上使用 CAU 工具。    
  
下表摘要兩種更新 CAU 模式 CAU 功能安裝需求。  
  
|安裝的元件|Self\ 更新模式|Remote\ 更新模式|  
|-----------------------|-----------------------|-------------------------|  
|容錯功能|所需的所有叢集節點|所需的所有叢集節點|  
|容錯移轉叢集工具|所需的所有叢集節點|必要 remote\ 更新電腦上<br />-所需的所有叢集節點上執行[儲存-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet|  
|CAU 叢集的角色|需要|不需要|  
  
## <a name="obtain-an-administrator-account"></a>取得管理員  
下列系統管理員需求，都是為了使用 CAU 功能。  
  
-   預覽，或使用 CAU 使用者介面套用更新動作 \(UI\) 或 cmdlet 叢集更新，您必須使用核對的所有叢集節點都具有本機系統管理員權限。 如果 account 不會在每個節點具有不足的權限，您會提示叢集更新視窗中提供必要的認證，當您在執行這些動作。 使用[更新叢集](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)cmdlet，您可以做為 cmdlet 參數提供必要的認證。  
  
-   如果您使用 account 不會有本機系統管理員權限與權限叢集節點上登入 CAU 使用 remote\ 更新模式中，您必須 CAU 工具系統管理員身分執行使用本機系統管理員 account 更新協調器在電腦上，或使用 account 的**後驗證模擬 client**使用者權利。 
  
-   若要執行 CAU 最佳做法分析，您必須使用 account 叢集節點上的系統管理員權限，以及用來執行的電腦上的本機系統管理員權限的[測試-CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet 或分析叢集更新整備使用叢集更新視窗。 如需詳細資訊，請查看[更新整備測試叢集](#BKMK_BPA)。  
  
## <a name="verify-the-cluster-configuration"></a>請確認叢集設定  
以下是使用 CAU 支援更新容錯移轉叢集一般需求。 節點上的遠端管理的需求額外的設定會列在[設定的遠端管理節點](#BKMK_NODE_CONFIG)本主題中的更新版本。  
  
-   有仲裁，必須 online 充足的叢集節點。  
  
-   所有叢集節點必須都是相同的 Active Directory domain。  
  
-   使用 DNS 網路，必須解析叢集名稱。  
  
-   如果 CAU 用於 remote\ 更新模式時，更新協調器的電腦必須網路連接到容錯移轉叢集節點中，並必須是相同的 Active Directory 容錯移轉叢集網域中。  
  
-   叢集服務執行所有叢集節點。 預設所有叢集節點上已安裝這項服務，且已設定為自動 [開始]。  
  
-   若要使用 PowerShell pre\-更新版或更新 post\ 指令碼 CAU 更新執行時，確保所有叢集節點上已安裝的指令碼或的檔案共用高度可用的網路上存取所有節點。 如果指令碼儲存到檔案共用網路，設定資料夾朗讀每個人的權限的群組。  
  
## <a name="BKMK_NODE_CONFIG"></a>設定節點的遠端管理  
若要用於叢集更新，必須的遠端管理所有節點叢集的都設定。 根據預設，只有工作，必須執行設定節點的遠端管理是[讓防火牆規則允許自動重新開機以](#BKMK_FW)。 

下表列出完整的遠端管理的需求，以便在您的環境出現的預設值。

這些需求的除了安裝需求[安裝容錯功能與容錯移轉叢集工具](#BKMK_REQ_CLUS)和一般叢集需求本主題中的前一節中所述。  
  
|需求|預設的狀態|Self\ 更新模式|Remote\ 更新模式|  
|---------------|---|-----------------------|-------------------------|  
|[讓允許自動重新開機防火牆規則](#BKMK_FW)|停用|使用防火牆是否需要所有叢集節點|使用防火牆是否需要所有叢集節點|  
|[讓 Windows 管理檢測](#BKMK_WMI)|支援|所需的所有叢集節點|所需的所有叢集節點|  
|[讓 Windows PowerShell 3.0 或 4.0 及 Windows PowerShell 遠端](#BKMK_PS)|支援|所需的所有叢集節點|所需的所有叢集節點執行下列動作：<br /><br />-[儲存-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-在更新執行 PowerShell pre\ 更新及更新 post\ 指令碼<br />-測試的更新整備使用叢集更新視窗叢集或[Test\-CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) Windows PowerShell cmdlet|  
|[安裝.NET Framework 4.6 或 4.5](#BKMK_NET)|支援|所需的所有叢集節點|所需的所有叢集節點執行下列動作：<br /><br />-[儲存-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-在更新執行 PowerShell pre\ 更新及更新 post\ 指令碼<br />-測試的更新整備使用叢集更新視窗叢集或[Test\-CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) Windows PowerShell cmdlet|  

### <a name="BKMK_FW"></a>讓允許自動重新開機防火牆規則  
套用更新之後，允許自動重新開機 \（如果安裝的更新需要 restart\），如果 Windows 防火牆或 non\ Microsoft 防火牆位於叢集節點上使用，必須將防火牆規則支援下列流量一種可以讓每個節點上：  
  
-   通訊協定：TCP  
  
-   方向：輸入  
  
-   計畫：wininit.exe  
  
-   連接埠：RPC 動態連接埠  
  
-   個人檔案：網域  
  
如果 Windows 防火牆叢集節點上使用時，您可以藉由讓**遠端關機**上的每個節點叢集 Windows 防火牆規則群組。 當您使用叢集更新視窗適用的更新，並設定 self\ 更新選項]**遠端關機**Windows 防火牆規則群組自動支援的每個節點叢集上。  
  
> [!NOTE]  
> **遠端關機**時，它會使用群組原則」設定設定為 Windows 防火牆衝突無法功能的 Windows 防火牆規則群組。    
  
**遠端關機**防火牆規則群組也支援藉由**– EnableFirewallRules**參數，執行下列 CAU cmdlet 時：[新增-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)、[叫用-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)，和[SetCauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)。  
  
下列 PowerShell 範例另一種方法來讓叢集節點上的自動重新開機。  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>讓 Windows 管理檢測 (WMI) 
使用 Windows 管理檢測 \(WMI\) 的遠端管理所有叢集節點必須都設定。 這是預設支援。  
  
若要手動讓遠端管理，執行下列動作：  
  
1.  [服務] 主控台，在 [開始] **Windows 遠端管理**服務，並為開機輸入**自動**。  
  
2.  執行[設定為 WSManQuickConfig](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.wsman.management/set-wsmanquickconfig) cmdlet 或從提升權限的命令的執行下列命令提示：  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
請支援 WMI 遠端，如果 Windows 防火牆位於叢集節點上使用，輸入的防火牆規則適用於**Windows 遠端管理 \(HTTP\-In\)**必須將在每個節點支援。  根據預設，被讓本規則。  
  
### <a name="BKMK_PS"></a>讓 Windows PowerShell 及 Windows PowerShell 遠端  
若要讓 self\ 更新模式和 remote\ 更新模式中的特定 CAU 功能，必須安裝和連接到執行遠端命令所有叢集節點 PowerShell。 根據預設，PowerShell 是安裝及遠端支援。  
  
若要讓遠端 PowerShell，使用下列其中一個下列方法：  
  
-   執行[讓-PSRemoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) cmdlet。  
  
-   設定適用於 Windows 遠端管理 \(WinRM\) domain\ 層級群組原則設定。  
  
如需關於遠端 PowerShell 的詳細資訊，請查看[about\_Remote\_Requirements](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_remote_requirements)。  
  
### <a name="BKMK_NET"></a>安裝.NET Framework 4.6 或 4.5  
為了讓 self\ 更新模式和 remote\ 更新模式、.NET Framework 4.6、或（在 Windows Server 2012 R2) 上的.NET Framework 4.5 特定 CAU 功能必須安裝所有叢集節點。 根據預設，安裝 NET Framework。  

若要安裝.NET Framework 4.6（或 4.5）使用 PowerShell 尚未安裝，請使用下列命令：

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>最好的作法建議使用叢集更新 
  
### <a name="BKMK_BP_WUA"></a>建議套用 Microsoft 更新

我們建議，當您開始使用 CAU 套用預設值的更新**Microsoft.WindowsUpdatePlugin** plug\ 中叢集上，您停止使用其他方法叢集節點上安裝來自 Microsoft 的軟體更新。  
  
> [!CAUTION]  
> 使用方法的自動更新個人節點結合 CAU \（修正的時間 schedule\) 可能會造成無法預期的結果，服務與計畫的中斷包括被迫中斷作業。  
  
我們建議您依照下列指導方針操作︰  
  
-   為了獲得最佳的結果，建議您停用設定為自動更新，叢集節點，例如透過 [設定自動更新使用群組原則設定的設定或 [控制台] 中。  
  
    > [!CAUTION]  
    > 自動安裝更新叢集節點上可能會干擾安裝的更新，CAU，可能會造成 CAU 失敗。  
  
    如果需要下列設定自動更新的相容 CAU，因為系統管理員可以控制更新的安裝時間：  
  
    -   設定之前先下載更新通知通知安裝之前  
  
    -   設定為自動下載更新，以及通知安裝之前  
  
    不過，如果自動更新會下載更新為 CAU 更新執行一次更新執行可能需要較長的時間來完成。  
  
-   例如，Windows Server Update Services \(WSUS\) 套用自動更新不設定更新系統 \（上修正的時間 schedule\) 叢集節點。  
  
-   若要使用的相同更新的來源，例如 WSUS 伺服器、Windows Update，或 Microsoft Update 所有叢集節點應該而言都設定。  
  
-   如果您使用的組態管理系統適用的軟體更新到網路上的電腦，請叢集節點排除所有所需或 [自動更新。 組態管理系統範例包括 Microsoft System Center Configuration Manager 2007 與 Microsoft System Center 一樣管理員 2008 年。  
  
-   如果內部軟體 distribution 伺服器 \ (例如，WSUS servers\) 用來包含和部署更新，請確定那些伺服器正確找出叢集節點核准的更新。  
  
#### <a name="BKMK_PROXY"></a>適用於 Microsoft 分公司案例更新  
若要下載 Microsoft update 來自 Microsoft 的更新或 Windows 更新，叢集節點特定分公司案例中，您可能需要在每個節點設定本機系統帳號 proxy 設定。 例如，您可能需要執行此動作，如果您的分支 office 叢集存取 Microsoft Update 或 Windows Update 使用本機 proxy 伺服器以下載更新。  
  
如有需要，設定 WinHTTP proxy 指定本機 proxy 伺服器，並設定本機位址例外每個節點上 \（也就是本機 addresses\ 略過清單）。 若要這樣做，您可以從提升權限的命令提示字元中每個叢集節點上執行下列命令：  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
位置 <*ProxyServerFQDN*> proxy 伺服器的完整的網域名稱和 <*連接埠*> 是要通訊的連接埠（通常是連接埠 443）。  
  
例如，若要設定 WinHTTP proxy 本機系統 account 指定的 proxy 伺服器設定*MyProxy.CONTOSO.com*、連接埠 443 與當地的地址例外，輸入下列命令：  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>建議使用 Microsoft.HotfixPlugin  
  
-   我們建議您設定的權限 hotfix 根資料夾只本機系統管理員用來儲存這些檔案的電腦上限制存取寫入 hotfix 設定檔中。 這可協助防止竄改未經授權的使用者危及容錯移轉叢集的功能，在套用 hotfix 時，這些檔案。  
  
-   為了確保用於存取 hotfix 根資料夾伺服器訊息區塊 \(SMB\) 連接的資料的完整性，您應該在 SMB 共用資料夾中，設定 SMB 加密是否可以將其設定。 **Microsoft.HotfixPlugin**需要 SMB 登入或 SMB 加密設定可協助您確保資料的完整性 SMB 連接。 
  
    如需詳細資訊，請查看[限制存取的 hotfix 根資料夾和 hotfix 設定檔以](cluster-aware-updating-plug-ins.md#BKMK_ACL)。
  
### <a name="additional-recommendations"></a>其他建議  
  
-   若要避免干擾 CAU 更新執行排定可能會在此同時，不要期間維護 windows 排程叢集名稱與 virtual 電腦物件的變更密碼。  
  
-   您應該設定 pre\ 更新及更新 post\ 指令碼未經授權的使用者會儲存在網路共用資料夾，以避免潛在竄改這些檔案上的適當權限。  
  
-   若要設定 CAU self\ 更新模式，virtual 電腦物件必須在 Active Directory 中建立 \(VCO\) CAU 叢集角色。 CAU 可以建立此物件會自動新增 CAU 叢集的角色，同時容錯移轉叢集有不足權限。 不過，某些組織的安全性原則，因為它可能需要在 Active Directory 物件預先分段準備。 若要這樣做為程序，請查看[步驟預先設置負責叢集角色](http://go.microsoft.com/fwlink/p/?LinkId=237624)。  
  
-   要儲存的容錯上重複使用更新執行設定類似更新需求 IT 組織中，您可以建立更新執行設定檔。 此外，根據更新模式中，您可以儲存及管理更新執行設定檔上的所有更新協調器的遠端電腦或容錯可以存取檔案共用。 如需詳細資訊，請查看[進階選項]，然後 CAU 對更新執行設定檔](cluster-aware-updating-options.md)。  
  
## <a name="BKMK_BPA"></a>更新整備測試叢集  
您可以在執行測試是否容錯移轉叢集 CAU 最佳做法分析 \(BPA\) 型號和網路環境符合許多套用 CAU 的軟體更新的需求。 許多測試核取 [使用預設值 plug\ 在套用 Microsoft 更新整備的環境**Microsoft.WindowsUpdatePlugin**。  
  
> [!NOTE]  
> 您可能需要獨立驗證您的環境叢集可套用軟體的更新以外 plug\ 中使用**Microsoft.WindowsUpdatePlugin**。 如果您使用 non\ Microsoft plug\ 中，例如您的硬體製造商所提供的其中一個，請發行者連絡的詳細資訊。  
  
您可以執行 BPA 下列兩方面：  
  
1.  選取 [**更新整備分析叢集**中 CAU 主機。 BPA 完成整備測試之後，就會出現測試報告。 如果叢集節點上偵測到問題的特定問題與節點出現問題的位置都會，因此您可能需要修正的動作。 測試可能需要幾分鐘的時間來完成。  
  
2.  執行[測試-CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet。 您可以執行 cmdlet 在本機或遠端電腦已安裝容錯移轉叢集模組適用於 Windows PowerShell（容錯移轉叢集工具的一部分）。 您也可以執行 cmdlet 容錯移轉叢集節點上。  
  
> [!NOTE]  
> -   您必須使用帳號叢集節點上的系統管理員權限和本機系統管理員權限，用來執行的電腦上的**Test\-CauSetup** cmdlet 或分析叢集更新整備使用叢集更新視窗。 若要執行使用叢集更新視窗的測試，您必須必要的認證的電腦登入。  
> -   測試假設，CAU 工具，可用來預覽和適用的軟體更新執行相同的電腦，並使用的相同使用者認證為用來測試叢集更新整備。  
  
> [!IMPORTANT]  
> 建議您測試的更新整備下列情形叢集︰  
>   
> -   使用第一次 CAU 套用軟體更新。  
> -   之後將節點新增至叢集或執行叢集需要執行驗證叢集精靈中的其他硬體變更。  
> -   在您變更更新的來源，或變更更新設定之後 \(other than CAU\)，可能會影響更新節點上的應用程式。  
  
### <a name="tests-for-cluster-updating-readiness"></a>用於叢集更新整備測試  
下表列出叢集更新整備測試、一些常見的問題，以及解析度步驟。  
  
|測試|可能的問題與影響|解析度步驟|  
|--------|-------------------------------|--------------------|  
|容錯移轉叢集必須使用|無法解析容錯移轉叢集名稱或一或多個叢集節點無法存取。 BPA 無法執行叢集整備測試。|-檢查指定期間執行 BPA 叢集的名稱。<br />為確保叢集的所有節點 online 和執行。<br />-查看的驗證設定精靈可以順利執行容錯移轉叢集上。|  
|必須能透過 WMI 的遠端管理支援容錯移轉叢集節點|一或多個容錯移轉叢集節點不是藉由 Windows 管理檢測 \(WMI\) 支援的遠端管理。 如果未的遠端管理設定節點 CAU 無法更新叢集節點。|請確定所有容錯移轉叢集節點才透過 WMI 的遠端管理。 如需詳細資訊，請查看[設定的遠端管理節點](#BKMK_NODE_CONFIG)本主題中。|  
|遠端 PowerShell 應該會在每個容錯移轉叢集節點支援|PowerShell 無法安裝或上一個或多個容錯移轉叢集節點遠端不支援。 CAU 無法 self\ 更新模式設定，或使用某些功能 remote\ 更新模式。|確定已安裝所有叢集節點 PowerShell，並遠端支援。<br /><br />如需詳細資訊，請查看[設定的遠端管理節點](#BKMK_NODE_CONFIG)本主題中。|  
|容錯移轉叢集版本|Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 中容錯移轉叢集一或多個節點不會執行。 CAU 無法更新容錯移轉叢集。|確認已在 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 執行期間執行 BPA 指定容錯移轉叢集。<br /><br />如需詳細資訊，請查看[確認叢集設定](#BKMK_REQ_CLUS)本主題中。|  
|所有容錯移轉叢集節點必須安裝所需的版本的.NET Framework 及 Windows PowerShell|.NET framework 4.6、4.5 或 Windows PowerShell 尚未安裝一或多個叢集節點上。 某些 CAU 功能可能無法運作。|如果需要的話，請確定.NET Framework 4.6 或 4.5 及 Windows PowerShell 安裝所有叢集節點上。<br /><br />如需詳細資訊，請查看[設定的遠端管理節點](#BKMK_NODE_CONFIG)本主題中。|  
|叢集服務執行所有叢集節點|叢集服務不一或多個節點上執行。 CAU 無法更新容錯移轉叢集。|-確保叢集服務 \(clussvc\) 開始叢集，在所有節點上並設定為自動開始。<br />-查看的驗證設定精靈可以順利執行容錯移轉叢集上。<br /><br />如需詳細資訊，請查看[確認叢集設定](#BKMK_REQ_CLUS)本主題中。|  
|自動更新不必須設定為自動安裝更新的任何錯誤後移轉叢集節點|在至少一個錯誤後移轉叢集] 節點，自動更新被設定為自動安裝該節點 Microsoft update。 與其他更新程式方法結合 CAU 可能會導致未計畫當機或無法預期的結果。|如果 Windows Update 功能一或多個叢集節點上設定自動更新，請確定未自動更新設定為自動安裝更新。<br /><br />如需詳細資訊，請查看[適用於套用更新 Microsoft 建議](#BKMK_BP_WUA)。|  
|容錯移轉叢集節點應該使用的相同更新的來源|容錯移轉叢集節點一或多個更新的來源使用 Microsoft 更新中的其餘部分節點不同的設定。 更新可能不會套用而言叢集節點上的 CAU。|確定要使用的相同更新的來源，例如 WSUS 伺服器、Windows Update，或 Microsoft Update 設定的每個節點叢集。<br /><br />如需詳細資訊，請查看[適用於套用更新 Microsoft 建議](#BKMK_BP_WUA)。|  
|應該會容錯移轉叢集中每個節點上支援，可讓遠端關機防火牆規則|一或多個容錯移轉叢集節點不需要防火牆規則支援，可讓遠端關機，或群組原則設定可防止此規則會支援。 更新執行適用於需要重新節點自動更新，可能無法完成正常運作。|如果 Windows 防火牆或 non\ Microsoft 防火牆位於叢集節點上使用，設定，可讓遠端關機防火牆規則。<br /><br />如需詳細資訊，請查看[讓防火牆規則允許自動重新開機以](#BKMK_FW)本主題中。|  
|每個容錯移轉叢集節點上的 proxy 伺服器設定應設本機 proxy 伺服器|一或多個錯誤後移轉叢集節點具有正確的 proxy 伺服器設定。<br /><br />如果使用本機 proxy 伺服器，每個節點上的 proxy 伺服器設定必須設定正確叢集存取 Microsoft 更新或 Windows 更新。|確定 WinHTTP proxy 設定的每個節點叢集上設定本機 proxy 伺服器需要。 如果 proxy 伺服器不是在您的環境中使用，可以忽略此警告。<br /><br />如需詳細資訊，請查看[更新分公司案例適用於](#BKMK_PROXY)本主題中。|  
|應該安裝以便 self\ 更新模式容錯移轉叢集上 CAU 叢集的角色|這個錯誤後移轉叢集上並未安裝 CAU 叢集的角色。 用於叢集 self\ 更新需要此角色。|若要使用 CAU self\ 更新模式，新增 CAU 叢集的角色容錯移轉叢集上的其中之一下列方式：<br /><br />-執行[新增-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) PowerShell cmdlet。<br />-選取**設定叢集 self\ 更新選項**叢集更新視窗中的動作。|  
|CAU 叢集的角色應該容錯移轉叢集上支援，可讓 self\ 更新模式|停用 CAU 叢集的角色。 例如，CAU 叢集的角色尚未安裝，或已停用來使用[Disable\-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/disable-cauclusterrole) PowerShell cmdlet。 用於叢集 self\ 更新需要此角色。|若要使用 CAU self\ 更新模式，讓 [CAU 叢集的角色此錯誤後移轉叢集上其中一個下列方式：<br /><br />-執行[讓-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/enable-cauclusterrole) PowerShell cmdlet。<br />-選取**設定叢集 self\ 更新選項**叢集更新視窗中的動作。|  
|必須將所有容錯移轉叢集節點登記設定的 CAU plug\ 單元 self\ 更新模式|這個錯誤後移轉叢集一或多個節點 CAU 叢集的角色無法存取 CAU plug\ 中模組 self\ 更新選項中設定。 Self\ 更新執行可能會失敗。|-確認設定的 CAU plug\ 在已安裝所有叢集節點提供 plug\ 中 CAU product 安裝程序。<br />-執行[Register\-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin)以 plug\ 在必要的叢集節點上登記 PowerShell cmdlet。|  
|所有容錯移轉叢集節點應該會有相同的且已 CAU plug\ 增益集|如果 plug\ 單元以供更新執行設定的變更，並不適用於所有叢集節點，可能會失敗 self\ 更新執行。|-確認設定的 CAU plug\ 在已安裝所有叢集節點提供 plug\ 中 CAU product 安裝程序。<br />-執行[Register\-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin)以 plug\ 在必要的叢集節點上登記 PowerShell cmdlet。|  
|[設定] 更新執行選項必須有效|設定的這個錯誤後移轉叢集更新執行選項與 self\ 更新排程不完整，或不正確。 Self\ 更新執行可能會失敗。|設定有效 self\ 更新排程和更新執行選項。 例如，您可以使用[Set\-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)來設定 CAU PowerShell cmdlet 叢集角色。|  
|在至少兩個容錯移轉叢集節點必須 CAU 叢集角色的擁有者|更新執行啟動 self\ 更新模式中將會失敗，因為 CAU 叢集的角色不需要移至可能的擁有者節點。|使用容錯移轉叢集工具，以確保所有叢集節點都設定為可能 CAU 擁有者叢集角色。 此為預設設定。||  
|所有容錯移轉叢集節點必須能存取 Windows PowerShell 指令碼|並非所有可能擁有者節點 CAU 叢集角色的可以存取設定的 Windows PowerShell pre\ 更新及更新 post\ 指令碼。 將會失敗 self\ 更新執行。|確保所有可能擁有者節點 CAU 叢集角色的擁有權限存取設定的 PowerShell pre\ 更新及更新 post\ 指令碼。|  
|所有容錯移轉叢集節點應該使用相同的 Windows PowerShell 指令碼|並非所有可能擁有者節點 CAU 叢集角色的使用相同的指定的 Windows PowerShell pre\ 更新和更新 post\ 指令碼。 Self\ 更新執行可能會失敗，或者顯示未預期的行為。|請確定 CAU 叢集角色的所有可能擁有者節點使用的相同 PowerShell pre\ 更新及更新 post\ 指令碼。|  
|對更新執行指定 WarnAfter 設定應小於 StopAfter 設定|指定的 CAU 更新執行逾時值進行警告逾時無效。 更新執行可能會之前取消可以產生警告事件登入。|在更新執行選項]，設定**WarnAfter**選項，值小於**StopAfter**選項值。|  
  
## <a name="see-also"></a>也了  
  
-   [叢集更新概觀](cluster-aware-updating.md)
  
