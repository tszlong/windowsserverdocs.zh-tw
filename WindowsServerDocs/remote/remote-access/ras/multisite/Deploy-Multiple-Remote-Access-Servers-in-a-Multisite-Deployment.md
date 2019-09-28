---
title: 以多站台部署方式部署多個遠端存取伺服器
description: 本主題是在 Windows Server 2016 的多網站部署中部署多部遠端存取服務器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da23f3082e1d97f1bcfbee7365b863d29ba2d020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404497"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>以多站台部署方式部署多個遠端存取伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與遠端存取服務（RAS） VPN 結合成一個遠端存取角色。 在多個企業案例中，我們可以部署「遠端存取」。 本總覽提供在多網站設定中部署遠端存取服務器的企業案例簡介。  
  
## <a name="BKMK_OVER"></a>案例描述  
在多網站部署中，會部署兩個或更多部遠端存取服務器或伺服器叢集，並將其設定為單一位置或散佈地理位置中的不同進入點。 在單一位置部署多個進入點，可允許伺服器的冗余，或與現有網路架構的遠端存取服務器一致。 依地理位置部署可確保有效率地使用資源，因為遠端用戶端電腦可以使用最接近它們的進入點連線到內部網路資源。 跨多網站部署的流量可以透過外部全域負載平衡器來散發和平衡。  
  
多網站部署支援執行 Windows 10、Windows 8 或 Windows 7 的用戶端電腦。 執行 Windows 10 或 Windows 8 的用戶端電腦會自動識別進入點，或使用者可以手動選取進入點。 自動指派會依照下列優先權順序進行：  
  
1.  使用使用者手動選取的進入點。  
  
2.  使用外部全域負載平衡器所識別的進入點（如果已部署的話）。  
  
3.  使用用戶端電腦使用自動探查機制所識別的最接近的進入點。  
  
支援執行 Windows 7 的用戶端必須在每個進入點上手動啟用，且不支援選取這些用戶端的進入點。  
  
## <a name="prerequisites"></a>必要條件  
開始部署這個案例之前，請先檢閱這份重要需求清單：  
  
-   [部署具有 Advanced 設定的單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)必須先部署，然後再部署多網站。  
  
-   Windows 7 用戶端一律會連接到特定網站。 他們將無法根據用戶端的位置（不同于 Windows 10、8或8.1 用戶端）連接到最接近的網站。  
  
-   不支援在 DirectAccess 管理主控台以外或使用 PowerShell Cmdlet 來變更原則。  
  
-   必須部署公開金鑰基礎結構。  
  
    如需詳細資訊，請參閱：@no__t 0Test Lab Guide 迷你模組：Windows Server 2012 的基本 PKI。 ](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   公司網路必須啟用 IPv6。 如果您使用 ISATAP，則應該將它移除，然後使用原生的 IPv6。  
  
## <a name="in-this-scenario"></a>在這個案例中  
多網站部署案例包含幾個步驟：  
  
1. [使用 [advanced] 設定部署單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 在設定多網站部署之前，必須先部署具有 advanced 設定的單一遠端存取服務器。  
  
2. [規劃多網站部署](plan/Plan-a-Multisite-Deployment.md)。 若要從單一伺服器建立多網站部署，需要一些額外的規劃步驟，包括符合多網站必要條件的規範，以及規劃 Active Directory 安全性群組、群組原則物件（Gpo）、DNS 和用戶端設定。  
  
3. [設定多網站部署](configure/Configure-a-Multisite-Deployment.md)。 這包含幾個設定步驟，包括準備 Active Directory 基礎結構、設定現有的遠端存取服務器，以及新增多部遠端存取服務器做為多網站部署的進入點。  
  
4. 針對[多網站部署進行疑難排解](troubleshoot/Troubleshoot-a-Multisite-Deployment.md)。 此疑難排解章節說明在多網站部署中部署遠端存取時，可能發生的數個最常見的錯誤。
  
## <a name="BKMK_APP"></a>實際應用  
多網站部署提供下列各項：  
  
-   改善的效能-多網站部署可讓用戶端電腦使用遠端存取來存取內部資源，以使用最接近且最適合的進入點進行連線。 用戶端會有效率地存取內部資源，並改善透過 DirectAccess 路由傳送的用戶端網際網路要求速度。 您可以使用外部全域負載平衡器來平衡進入點之間的流量。  
  
-   易於管理-多網站可讓系統管理員將遠端存取部署與 Active Directory 網站部署保持一致，以提供簡化的架構。 您可以輕鬆地在進入點伺服器或叢集之間設定共用設定。 遠端存取設定可以從部署中的任何伺服器進行管理，或從遠端使用遠端伺服器管理工具（RSAT）。 此外，您可以從單一遠端存取管理主控台監視整個多網站部署。  
  
## <a name="BKMK_NEW"></a>此案例中包含的角色和功能  
下表列出此案例中使用的角色和功能。  
  
|角色/功能|如何支援本案例|  
|---------|-----------------|  
|遠端存取角色|這個角色是利用伺服器管理員主控台安裝和解除安裝。 它包含 DirectAccess (以前是 Windows Server 2008 R2 的功能)、 路由及遠端存取服務 (RRAS，以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<br /><br />-DirectAccess 與路由及遠端存取服務（RRAS） VPN-DirectAccess 和 VPN 會在 [遠端存取管理] 主控台中一起管理。<br />-RRAS 路由-在舊版路由及遠端存取主控台中管理 RRAS 路由功能。<br /><br />相依性如下所示：<br /><br />-Internet Information Services （IIS）網頁伺服器-設定網路位置伺服器和預設 Web 探查時，需要這項功能。<br />-Windows 內部資料庫-用於遠端存取服務器上的本機帳戶處理。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-安裝遠端存取角色時，預設會將它安裝在遠端存取服務器上，並支援遠端管理主控台使用者介面。<br />-您可以選擇性地將它安裝在未執行遠端存取服務器角色的伺服器上。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取 GUI 和命令列工具<br />-適用于 Windows PowerShell 的遠端存取模組<br /><br />依存項目包括：<br /><br />-群組原則管理主控台<br />-RAS 連線管理員系統管理元件（CMAK）<br />-Windows PowerShell 3。0<br />-圖形化管理工具與基礎結構|  
  
## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
-   至少要將兩部遠端存取電腦收集到多網站部署。   
  
-   為了測試此案例，至少需要一部執行 Windows 8 的電腦，並設定為 DirectAccess 用戶端。 若要測試執行 Windows 7 之用戶端的案例，至少需要一部執行 Windows 7 的電腦。  
  
-   若要平衡進入點伺服器之間的流量負載，需要協力廠商外部全域負載平衡器。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
本案例的軟體需求如下：  
  
-   部署單一伺服器時的軟體需求。  
  
-   除了單一伺服器的軟體需求之外，還有幾個多網站特定的需求：  
  
    -   IPsec 驗證需求-在多網站部署中，DirectAccess 必須使用 IPsec 電腦憑證驗證來部署。 不支援使用遠端存取服務器做為 Kerberos proxy 來執行 IPsec 驗證的選項。 需要內部 CA 才能部署 IPsec 憑證。  
  
    -   Ip-HTTPs 和網路位置伺服器需求-ip-HTTPs 和網路位置伺服器所需的憑證必須由 CA 發行。 不支援使用自動發行且由遠端存取服務器自我簽署之憑證的選項。 憑證可以由內部 CA 或由協力廠商外部 CA 發行。  
  
    -   Active Directory 需求-至少需要一個 Active Directory 網站。 遠端存取服務器應該位於網站中。 為了加快更新時間，建議每個網站都有可寫入的網域控制站，但這不是必要的。  
  
    -   安全性群組需求-需求如下：  
  
        -   所有網域的所有 Windows 8 用戶端電腦都需要一個安全性群組。 建議為每個網域建立這些用戶端的唯一安全性群組。  
  
        -   設定為支援 Windows 7 用戶端的每個進入點都需要一個包含 Windows 7 電腦的唯一安全性群組。 建議每個網域中的每個進入點都有唯一的安全性群組。  
  
        -   電腦不可以包含在多個內含 DirectAccess 用戶端的安全性群組中。 如果用戶端是包含在多個群組中，則無法正常為每一個用戶端要求，進行名稱的解析工作。  
  
    -   GPO 需求-Gpo 可以在設定「遠端存取」之前手動建立，或在「遠端存取」部署期間自動建立。 需求如下：  
  
        -   每個網域都需要唯一的用戶端 GPO。  
  
        -   在進入點所在的網域中，每個進入點都需要伺服器 GPO。 因此，如果有多個進入點位於相同的網域中，網域中就會有多個伺服器 Gpo （每個進入點各一個）。  
  
        -   針對每個網域啟用 Windows 7 用戶端支援的每個進入點都需要唯一的 Windows 7 用戶端 GPO。  
  
## <a name="KnownIssues"></a>已知問題  
以下是設定多網站案例的已知問題：  
  
-   **相同 IPv4 子網中的多個進入點**。 在相同的 IPv4 子網中新增多個進入點會導致 IP 位址衝突訊息，而且不會如預期般設定進入點的 DNS64 位址。 當 IPv6 尚未部署在公司網路上的伺服器內部介面上時，就會發生此問題。 若要避免這個問題，請在所有目前和未來的遠端存取服務器上執行下列 Windows PowerShell 命令：  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   如果為 DirectAccess 用戶端指定的公用位址連線到遠端存取服務器時，在 NRPT 中包含尾碼，DirectAccess 可能無法如預期般運作。 確定 NRPT 具有公用名稱的豁免。 在多網站部署中，應新增所有進入點的公用名稱的豁免。 請注意，如果已啟用強制通道，則會自動新增這些豁免。 如果已停用 [強制通道]，則會移除它們。  
  
-   使用 Windows PowerShell Cmdlet **DAMultiSite**時，WhatIf 和 Confirm 參數不會有任何作用，而且會停用多網站，而且將會移除 Windows 7 gpo。  
  
-   當在多網站部署中使用 DCA 的 Windows 7 用戶端升級到 Windows 8 時，網路連接助理將無法運作。 在用戶端升級之前，可以使用下列 Windows PowerShell Cmdlet 修改 Windows 7 Gpo 來解決此問題：  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    若用戶端已升級，請將用戶端電腦移至 Windows 8 安全性群組。  
  
-   使用 Windows PowerShell Cmdlet **DAEntryPointDC**修改網域控制站設定時，如果指定的 ComputerName 參數是進入點的遠端存取服務器，而不是最後一個新增到多網站部署的專案，就會出現警告將會顯示，表示指定的伺服器在下一次的原則重新整理之前不會更新。 在 [**遠端存取管理] 主控台**的 [**儀表板**] 中，您可以使用 [設定**狀態**] 看到未更新的實際伺服器。 這不會造成任何功能上的問題，不過，您可以在未更新的伺服器上執行**gpupdate/force** ，以立即更新設定狀態。  
  
-   當多網站部署在僅 IPv4 的公司網路時，變更內部網路 IPv6 首碼也會變更 DNS64 位址，但不會更新允許 DNS 查詢至 DNS64 服務的防火牆規則上的位址。 若要解決此問題，請在變更內部網路 IPv6 首碼之後，執行下列 Windows PowerShell 命令：  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   如果在現有的 ISATAP 基礎結構存在時部署 DirectAccess，則移除屬於 ISATAP 主機的進入點時，將會從 NRPT 中所有 DNS 尾碼的 DNS 伺服器位址移除 DNS64 服務的 IPv6 位址。  
  
    若要解決此問題，請在 [**基礎結構伺服器安裝程式**] 的 [ **dns** ] 頁面上，移除已修改的 dns 尾碼，然後在 dns 伺服器**位址按一下 [** 偵測]，以正確的 dns 伺服器位址新增它們。對話方塊。  
  


