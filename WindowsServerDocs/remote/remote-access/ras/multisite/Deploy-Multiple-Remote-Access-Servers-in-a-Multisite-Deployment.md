---
title: 以多站台部署方式部署多個遠端存取伺服器
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ac2f6015-50a5-4909-8f67-8565f9d332a2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3091c770fb9b207d82deaa571970bfeb44d17ee3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875879"
---
# <a name="deploy-multiple-remote-access-servers-in-a-multisite-deployment"></a>以多站台部署方式部署多個遠端存取伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 結合成單一的遠端存取角色 DirectAccess 和遠端存取服務 (RAS) VPN。 在多個企業案例中，我們可以部署「遠端存取」。 本概觀介紹企業案例部署在多站台設定中的遠端存取伺服器。  
  
## <a name="BKMK_OVER"></a>案例描述  
在站台部署兩個或多個遠端存取伺服器或伺服器叢集部署與設定為在單一位置，或分散各地的不同進入點。 部署在單一位置的多個進入點，可讓伺服器備援，或使用現有的網路架構的遠端存取伺服器的對齊方式。 依地理位置的部署會確保有效率地使用資源，因為遠端用戶端電腦可以連線到內部網路資源使用的是最靠近他們的進入點。 站台部署間的流量可以分散，並利用外部全域負載平衡器。  
  
站台部署支援執行 Windows 10，Windows 8 或 Windows 7 的用戶端電腦。 自動執行 Windows 10 或 Windows 8 用戶端電腦識別進入點，或使用者可以手動選取的進入點。 依下列優先順序，就會發生自動指派：  
  
1.  使用由使用者手動選取的進入點。  
  
2.  使用由外部全域負載平衡器，如果其中一個已部署的進入點。  
  
3.  使用最接近用戶端電腦使用自動探查機制所識別的進入點。  
  
支援執行 Windows 7 的用戶端必須在每個項目點上手動啟用，並不支援的這些用戶端的進入點選取範圍。  
  
## <a name="prerequisites"></a>先決條件  
開始部署這個案例之前，請先檢閱這份重要需求清單：  
  
-   [部署單一 DirectAccess 伺服器使用進階設定](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)站台部署之前，必須部署。  
  
-   Windows 7 用戶端一律都會連線至特定站台。 它們無法連線到最接近的站台 （不同於 Windows 10、windows 8 或 8.1 用戶端） 的用戶端的位置為基礎。  
  
-   不支援在 DirectAccess 管理主控台以外或使用 PowerShell Cmdlet 來變更原則。  
  
-   必須部署公開金鑰基礎結構。  
  
    如需詳細資訊，請參閱：[測試實驗室指南小單元：適用於 Windows Server 2012 的基本 PKI。](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   公司的網路必須啟用 IPv6。 如果您使用 ISATAP，則應該將它移除，然後使用原生的 IPv6。  
  
## <a name="in-this-scenario"></a>在這個案例中  
多站台的部署案例包含幾個步驟：  
  
1. [部署單一 DirectAccess 伺服器使用進階設定](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 設定站台部署之前，必須部署單一遠端存取伺服器使用進階設定。  
  
2. [規劃站台部署](plan/Plan-a-Multisite-Deployment.md)。 若要建立站台從單一伺服器部署許多額外的規劃步驟是必要的包括合規性的多站台的必要條件，以及規劃 Active Directory 安全性群組、 群組原則物件 (Gpo)、 DNS 及用戶端設定。  
  
3. [設定站台部署](configure/Configure-a-Multisite-Deployment.md)。 這包含數個設定步驟，包括準備 Active Directory 基礎結構，設定現有的遠端存取伺服器，並新增多部遠端存取伺服器做為多站台部署的進入點。  
  
4. [疑難排解多站台部署](troubleshoot/Troubleshoot-a-Multisite-Deployment.md)。 此疑難排解章節說明一些最常見多站台部署中部署遠端存取時可能發生的錯誤。
  
## <a name="BKMK_APP"></a>實際的應用程式  
站台部署提供下列功能：  
  
-   改善的效能的多站台部署可讓用戶端存取內部資源，使用 「 遠端存取連線使用的最接近且最適合用來進入點的電腦。 用戶端有效地存取內部資源，進而改善用戶端的網際網路要求路由傳送透過 DirectAccess 的速度。 可以使用外部全域負載平衡器平衡流量到進入點。  
  
-   簡化的管理 Multisite 可讓系統管理員根據 Active Directory 站台部署，提供簡化的架構來部署 「 遠端存取。 在項目點的伺服器或叢集時，可以輕鬆設定共用的設定。 可以從任何部署，或使用遠端伺服器管理工具 (RSAT) 從遠端伺服器管理遠端存取設定。 此外，可以監視整個多站台部署，從單一遠端存取管理主控台。  
  
## <a name="BKMK_NEW"></a>在此案例包含角色與功能  
下表列出的角色和功能，此案例中使用。  
  
|角色/功能|如何支援本案例|  
|---------|-----------------|  
|遠端存取角色|這個角色是利用伺服器管理員主控台安裝和解除安裝。 它包含 DirectAccess (以前是 Windows Server 2008 R2 的功能)、 路由及遠端存取服務 (RRAS，以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<br /><br />DirectAccess 與路由及遠端存取服務 (RRAS) VPN-管理 DirectAccess 和 VPN 一起在遠端存取管理主控台中。<br />-在舊版的路由及遠端存取主控台中管理 RRAS 路由-RRAS 路由功能。<br /><br />相依性如下所示：<br /><br />網際網路資訊服務 (IIS) Web Serve-這項功能，才能設定網路位置伺服器和預設 web 探查。<br />-Windows 內部 Database-Used 的遠端存取伺服器上的本機計量。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<br /><br />-它預設會安裝在遠端存取伺服器時的遠端存取角色安裝，並且可支援遠端管理主控台使用者介面。<br />-它可以選擇性地安裝在不執行 遠端存取伺服器角色的伺服器。 在這種情況下，它是用於從遠端管理那些執行 DirectAccess 和 VPN 的遠端存取電腦。<br /><br />遠端存取管理工具功能包含以下各項：<br /><br />-遠端存取 GUI 和命令列工具<br />-適用於 Windows PowerShell 遠端存取模組<br /><br />依存項目包括：<br /><br />群組原則管理主控台<br />-RAS Connection Manager Administration Kit (CMAK)<br />-   Windows PowerShell 3.0<br />圖形化管理工具和基礎結構|  
  
## <a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
-   蒐集到站台部署至少兩部遠端存取電腦。   
  
-   若要測試案例，至少一部電腦執行 Windows 8，並設定為 DirectAccess 用戶端則是必要項目。 若要測試案例執行 Windows 7 的用戶端至少一部執行 Windows 7 的電腦需要。  
  
-   進入點伺服器間的負載平衡流量，第三方外部全域負載平衡器則是必要項目。  
  
## <a name="BKMK_SOFT"></a>軟體需求  
本案例的軟體需求如下：  
  
-   部署單一伺服器時的軟體需求。  
  
-   除了在單一伺服器的軟體需求有幾個 multisite-特定的需求：  
  
    -   IPsec 驗證需求中必須使用 IPsec 電腦憑證驗證來部署多站台部署 DirectAccess。 不支援選項來執行 IPsec 驗證，使用遠端存取伺服器當作 Kerberos proxy。 內部 CA，才能部署 IPsec 憑證。  
  
    -   若為 IP-HTTPS 和網路位置伺服器需求-所需的憑證為 IP-HTTPS 和網路位置伺服器必須由 CA 發行。 不支援使用自動發行並由遠端存取伺服器自我簽署憑證的選項。 憑證可以在內部 CA 或由協力廠商外部 CA 所發行。  
  
    -   Active Directory 需求-須有至少一個 Active Directory 站台。 遠端存取伺服器應該位於站台。 更新時間加快，建議每個網站都有可寫入網域控制站，但這不是必要項目。  
  
    -   安全性群組需求-需求如下所示：  
  
        -   需要所有的網域中的所有 Windows 8 用戶端電腦的單一安全性群組。 建議您建立的每個網域的這些用戶端唯一的安全性群組。  
  
        -   每個進入點設定為支援 Windows 7 用戶端需要唯一的安全性群組，其中包含 Windows 7 電腦。 建議您每個網域中有唯一的安全性群組以每個進入點。  
  
        -   電腦不可以包含在多個內含 DirectAccess 用戶端的安全性群組中。 如果用戶端是包含在多個群組中，則無法正常為每一個用戶端要求，進行名稱的解析工作。  
  
    -   可以手動建立再設定遠端存取，或在遠端存取 」 部署期間自動建立 GPO 需求 Gpo。 需求如下所示：  
  
        -   需要針對每個網域的唯一用戶端 GPO。  
  
        -   以伺服器 GPO，才能進入點所在的網域中的每個項目點。 如果多個進入點位於相同網域中，將會有多個伺服器的網域中的 Gpo （一個用於每個進入點）。  
  
        -   唯一的 Windows 7 用戶端 GPO，才能啟用的每個網域的 Windows 7 用戶端支援每個進入點。  
  
## <a name="KnownIssues"></a>已知的問題  
下列是已知問題設定多站台的案例：  
  
-   **位於相同的 IPv4 子網路的多個進入點**。 在 IP 位址衝突訊息中，會在相同的 IPv4 子網路中新增多個進入點，並如預期般運作，將不會設定進入點的 DNS64 位址。 當沒有內部介面上公司網路的伺服器上部署 IPv6，就會發生此問題。 若要避免這個問題，所有目前和未來的遠端存取伺服器上執行下列 Windows PowerShell 命令：  
  
    ```  
    Set-NetIPInterface -InterfaceAlias <InternalInterfaceName> -AddressFamily IPv6 -DadTransmits 0  
    ```  
  
-   指定用於連線到遠端存取伺服器的 DirectAccess 用戶端的公用位址是否包含在 NRPT 尾碼，DirectAccess 可能無法如預期般運作。 請確定 NRPT 已豁免的公用名稱。 在多站台部署中，應該為公用的所有進入點名稱新增豁免。 請注意，如果強制通道啟用便會自動加入這些豁免。 如果已停用 強制通道，會移除這些項目。  
  
-   當使用 Windows PowerShell cmdlet**停用 DAMultiSite**、 WhatIf 和 Confirm 參數會有任何作用，以及多站台將會停用和 Windows 7 Gpo 將被移除。  
  
-   當使用 DCA 多站台部署中的 Windows 7 用戶端會升級為 Windows 8 時，網路連線助理將無法運作。 藉由修改 Windows 7 Gpo，使用下列 Windows PowerShell cmdlet，可以用戶端升級前解決此問題：  
  
    ```  
    Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
    Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
    ```  
  
    用戶端已經升級，然後將用戶端電腦移到 Windows 8 的安全性群組。  
  
-   修改網域控制站設定使用 Windows PowerShell cmdlet 時**Set-daentrypointdc**，如果指定的 ComputerName 參數是不是最後一個新增到多站台之進入點的遠端存取伺服器部署時，警告將會顯示指出指定的伺服器不會更新直到下次重新整理原則。 未更新的實際伺服器可以使用看到**組態狀態**中**儀表板**的**遠端存取管理主控台**。 這不會造成任何功能的問題，不過，您可以在其中執行**gpupdate /force**尚未更新來立即更新的設定狀態的伺服器上。  
  
-   在僅 IPv4 的公司網路中，變更內部部署多站台時網路也變更 DNS64 位址，IPv6 首碼，但不會更新防火牆規則允許至 DNS64 服務的 DNS 查詢上的位址。 若要解決此問題，請執行下列 Windows PowerShell 命令之後變更的內部網路 IPv6 首碼：  
  
    ```  
    $dns64Address = (Get-DAClientDnsConfiguration).NrptEntry | ?{ $_.DirectAccessDnsServers -match ':3333::1' } | Select-Object -First 1 -ExpandProperty DirectAccessDnsServers  
  
    $serverGpoName = (Get-RemoteAccess).ServerGpoName  
  
    $serverGpoDc = (Get-DAEntryPointDC).DomainControllerName  
  
    $gpoSession = Open-NetGPO -PolicyStore $serverGpoName -DomainController $serverGpoDc  
  
    Get-NetFirewallRule -GPOSession $gpoSession | ? {$_.Name -in @('0FDEEC95-1EA6-4042-8BA6-6EF5336DE91A', '24FD98AA-178E-4B01-9220-D0DADA9C8503')} |  Set-NetFirewallRule -LocalAddress $dns64Address  
  
    Save-NetGPO -GPOSession $gpoSession  
    ```  
  
-   如果現有的 ISATAP 基礎結構時存在，移除已 ISATAP 主機的進入點時，DirectAccess 已部署服務 DNS64 的 IPv6 位址將會移除從 NRPT 中的所有 DNS 尾碼的 DNS 伺服器位址。  
  
    若要解決這個問題，請在**基礎結構伺服器安裝**精靈**DNS**頁面上，移除已修改的 DNS 尾碼，並再次新增他們使用正確的 DNS 伺服器位址，依序按一下 [ **偵測**上**的 DNS 伺服器位址**] 對話方塊。  
  


