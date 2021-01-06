---
title: 以多站台部署方式部署多個遠端存取伺服器
description: 瞭解在多網站設定中部署遠端存取服務器的企業案例。
manager: brianlic
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 1d7ca177a2fe9483fef40d9f173b8ee4ce450db6
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946474"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>以多站台部署方式部署多個遠端存取伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與遠端存取服務 (RAS) VPN 合併為單一遠端存取角色。 在多個企業案例中，我們可以部署「遠端存取」。 本總覽提供在多網站設定中部署遠端存取服務器之企業案例的簡介。

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>案例描述
在多網站部署中，兩個或多個遠端存取服務器或伺服器叢集會部署並設定為單一位置中的不同進入點，或分散的地理位置。 在單一位置部署多個進入點，可允許伺服器的冗余，或與現有網路架構的遠端存取服務器一致。 依地理位置進行部署可確保有效率地使用資源，因為遠端用戶端電腦可以使用最接近它們的進入點連線至內部網路資源。 跨多網站部署的流量可以透過外部的全域負載平衡器散發及平衡。

多網站部署支援執行 Windows 10、Windows 8 或 Windows 7 的用戶端電腦。 執行 Windows 10 或 Windows 8 的用戶端電腦會自動識別進入點，或者使用者可以手動選取進入點。 自動指派會依照下列優先權順序進行：

1.  使用由使用者手動選取的進入點。

2.  如果已部署外部負載平衡器所識別的進入點，請使用它。

3.  使用用戶端電腦使用自動探查機制所識別的最接近進入點。

支援執行 Windows 7 的用戶端必須在每個進入點上手動啟用，而且不支援選取這些用戶端的進入點。

## <a name="prerequisites"></a>先決條件
開始部署這個案例之前，請先檢閱這份重要需求清單：

-   [部署具有 Advanced Settings 的單一 DirectAccess 伺服器時](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) ，必須先部署到多網站部署。

-   Windows 7 用戶端一律會連接到特定網站。 根據用戶端的位置而定，他們將無法連線到最接近的網站 () 的 Windows 10、8或8.1 用戶端不同。

-   不支援在 DirectAccess 管理主控台以外或使用 PowerShell Cmdlet 來變更原則。

-   必須部署公開金鑰基礎結構。

    如需詳細資訊，請參閱： [測試實驗室指南小單元：Windows Server 2012 的基本 PKI。](/answers/topics/windows-server-2012.html)

-   公司網路必須啟用 IPv6。 如果您使用 ISATAP，則應該將它移除，然後使用原生的 IPv6。

## <a name="in-this-scenario"></a>在這個案例中
多網站部署案例包含幾個步驟：

1. [使用 advanced Settings 部署單一 DirectAccess 伺服器](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 設定多網站部署之前，必須先部署具有 advanced settings 的單一遠端存取服務器。

2. [規劃多網站部署](plan/Plan-a-Multisite-Deployment.md)。 若要從單一伺服器建立多網站部署，需要進行一些額外的規劃步驟，包括符合多網站必要條件，以及規劃 Active Directory 安全性群組、群組原則物件 (Gpo) 、DNS 及用戶端設定。

3. [設定多網站部署](configure/Configure-a-Multisite-Deployment.md)。 這包含數個設定步驟，包括準備 Active Directory 基礎結構、設定現有的遠端存取服務器，以及將多個遠端存取服務器新增為多網站部署的進入點。

4. 針對[多網站部署進行疑難排解](troubleshoot/Troubleshoot-a-Multisite-Deployment.md)。 此疑難排解章節說明在多網站部署中部署遠端存取時可能發生的一些最常見錯誤。

## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用
多網站部署提供下列各項：

-   提高效能-多網站部署可讓用戶端電腦使用遠端存取來存取內部資源，以使用最接近且最適合的進入點進行連接。 用戶端可以有效率地存取內部資源，並改善透過 DirectAccess 路由傳送的用戶端網際網路要求的速度。 進入點間的流量可以使用外部的全域負載平衡器來平衡。

-   簡化管理-多網站可讓系統管理員將遠端存取部署與 Active Directory 網站部署一致，提供簡化的架構。 您可以輕鬆地在進入點伺服器或叢集之間設定共用設定。 遠端存取設定可以從部署中的任何伺服器進行管理，或使用遠端伺服器管理工具 (RSAT) 從遠端進行管理。 此外，您可以從單一遠端存取管理主控台監視整個多網站部署。

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>這個案例包含的角色與功能
下表列出此案例中使用的角色和功能。

|角色/功能|如何支援本案例|
|---------|-----------------|
|遠端存取角色|這個角色是利用伺服器管理員主控台安裝和解除安裝。 它包含 DirectAccess (以前是 Windows Server 2008 R2 的功能)、 路由及遠端存取服務 (RRAS，以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<p>-DirectAccess 與路由及遠端存取服務 (RRAS) VPN-DirectAccess 和 VPN 會在 [遠端存取管理] 主控台中一起管理。<br />-RRAS 路由-RRAS 路由功能是在舊版路由和遠端存取主控台中進行管理。<p>相依性如下所示：<p>-Internet Information Services (IIS) Web 服務器-設定網路位置伺服器和預設 Web 探查時，需要這項功能。<br />-Windows 內部 Database-Used 用於遠端存取服務器上的本機帳戶處理。|
|遠端存取管理工具功能|這個功能的安裝方式如下：<p>-安裝遠端存取角色時，預設會將它安裝在遠端存取服務器上，並支援遠端管理主控台使用者介面。<br />-您可以選擇性地將它安裝在未執行遠端存取服務器角色的伺服器上。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<p>遠端存取管理工具功能包含以下各項：<p>-遠端存取 GUI 和命令列工具<br />-Windows PowerShell 的遠端存取模組<p>依存項目包括：<p>-群組原則管理主控台<br />-RAS 連線管理員系統管理組件 (CMAK) <br />-Windows PowerShell 3。0<br />-圖形化管理工具和基礎結構|

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>硬體需求
本案例需要的硬體如下所示：

-   至少兩部要收集到多網站部署的遠端存取電腦。

-   為了測試案例，至少需要一部執行 Windows 8 且設定為 DirectAccess 用戶端的電腦。 若要針對執行 Windows 7 的用戶端測試此案例，至少需要一部執行 Windows 7 的電腦。

-   若要對進入點伺服器之間的流量進行負載平衡，則需要協力廠商外部全域負載平衡器。

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求
本案例的軟體需求如下：

-   部署單一伺服器時的軟體需求。

-   除了單一伺服器的軟體需求以外，還有一些多網站特定需求：

    -   IPsec 驗證需求-在多網站部署中，必須使用 IPsec 電腦憑證驗證部署 DirectAccess。 不支援使用遠端存取服務器做為 Kerberos proxy 執行 IPsec 驗證的選項。 部署 IPsec 憑證需要內部 CA。

    -   IP-HTTPS 和網路位置伺服器需求-ip-HTTPs 和網路位置伺服器所需的憑證必須由 CA 發出。 不支援使用自動發行的憑證，以及遠端存取服務器自我簽署的憑證選項。 憑證可以由內部 CA 或協力廠商外部 CA 發行。

    -   Active Directory 需求-至少需要一個 Active Directory 網站。 遠端存取服務器應該位於網站。 為了加快更新時間，建議每個網站都有一個可寫入的網域控制站，但這並非強制性。

    -   安全性群組需求-需求如下：

        -   所有網域的所有 Windows 8 用戶端電腦都需要單一安全性群組。 建議為每個網域建立這些用戶端的唯一安全性群組。

        -   每個設定為支援 Windows 7 用戶端的進入點都需要包含 Windows 7 電腦的唯一安全性群組。 建議每個網域中的每個進入點都有唯一的安全性群組。

        -   電腦不可以包含在多個內含 DirectAccess 用戶端的安全性群組中。 如果用戶端是包含在多個群組中，則無法正常為每一個用戶端要求，進行名稱的解析工作。

    -   GPO 需求-Gpo 可以在設定遠端存取之前手動建立，或在遠端存取部署期間自動建立。 需求如下：

        -   每個網域都需要唯一的用戶端 GPO。

        -   在進入點所在的網域中，每個進入點都需要伺服器 GPO。 因此，如果有多個進入點位於相同的網域中，就會有多個伺服器 Gpo (網域中) 的每個進入點。

        -   針對每個網域，每個啟用 Windows 7 用戶端支援的進入點都需要唯一的 Windows 7 用戶端 GPO。

## <a name="known-issues"></a><a name="KnownIssues"></a>已知問題
以下是設定多網站案例時的已知問題：

-   **相同 IPv4 子網中的多個進入點**。 在相同的 IPv4 子網中新增多個進入點將會導致 IP 位址衝突訊息，而且將不會如預期般設定進入點的 DNS64 位址。 當 IPv6 尚未部署在公司網路上伺服器的內部介面上時，就會發生此問題。 若要避免此問題，請在所有目前和未來的遠端存取服務器上執行下列 Windows PowerShell 命令：

    ```
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0
    ```

-   如果為 DirectAccess 用戶端指定用於連線至遠端存取服務器的公用位址具有在 NRPT 中包含的尾碼，則 DirectAccess 可能無法如預期般運作。 確定 NRPT 具有公用名稱的豁免。 在多網站部署中，應針對所有進入點的公用名稱新增豁免。 請注意，如果已啟用強制通道，則會自動新增這些豁免。 如果已停用強制通道，則會移除它們。

-   使用 Windows PowerShell Cmdlet **DAMultiSite** 時，WhatIf 和 Confirm 參數沒有任何作用，而且會停用多網站，而且將會移除 Windows 7 gpo。

-   當在多網站部署中使用 DCA 的 Windows 7 用戶端升級至 Windows 8 時，網路連線助理將無法運作。 您可以使用下列 Windows PowerShell Cmdlet 修改 Windows 7 Gpo，以在用戶端升級之前解決此問題：

    ```
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"
    ```

    如果用戶端已經升級，請將用戶端電腦移至 Windows 8 安全性群組。

-   使用 Windows PowerShell Cmdlet **set-daentrypointdc** 來修改網域控制站設定時，如果指定的 ComputerName 參數是在最後一個新增至多網站部署的進入點以外的遠端存取服務器，則會顯示警告，指出指定的伺服器將不會更新，直到下一次原則重新整理為止。 在 [**遠端存取管理] 主控台** 的 [**儀表板**] 中，您可以使用 [設定 **狀態**] 來看到未更新的實際伺服器 () 。 這並不會造成任何功能上的問題，不過，您可以在伺服器上執行 **gpupdate/force** ， (s) 未更新為立即更新設定狀態。

-   當多網站部署在僅限 IPv4 的公司網路時，變更內部網路 IPv6 首碼也會變更 DNS64 位址，但不會更新允許對 DNS64 服務進行 DNS 查詢的防火牆規則位址。 若要解決此問題，請在變更內部網路 IPv6 首碼之後，執行下列 Windows PowerShell 命令：

    ```
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers

    $serverGpoName = (Get-RemoteAccess).ServerGpoName

    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName

    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc

    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address

    Save-NetGPO -GPOSession $gpoSession
    ```

-   如果 DirectAccess 是在現有的 ISATAP 基礎結構存在時部署，則移除 ISATAP 主機的進入點時，會從 NRPT 中所有 DNS 尾碼的 DNS 伺服器位址移除 DNS64 服務的 IPv6 位址。

    若要解決此問題，請在 [ **基礎結構伺服器安裝程式** ] 的 [ **dns** ] 頁面上，移除已修改的 dns 尾碼， **然後按一下 [** **dns 伺服器位址** ] 對話方塊上的 [偵測]，以正確的 dns 伺服器位址新增這些尾碼。