---
title: 步驟 2 規劃進階 DirectAccess 部署
description: 本主題是指南部署單一 DirectAccess 伺服器使用進階設定的 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3bba28d4-23e2-449f-8319-7d2190f68d56
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c946d5bbdf6e8660aaa9e47ced44aed91cfb71da
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283458"
---
# <a name="step-2-plan-advanced-directaccess-deployments"></a>步驟 2 規劃進階 DirectAccess 部署

>適用於：Windows Server （半年通道），Windows Server 2016

在規劃 DirectAccess 基礎結構之後，使用 IPv4 和 IPv6 在單一伺服器上部署進階 DirectAccess 的下一步，就是規劃「遠端存取安裝精靈」的設定。  
  
|工作|描述|  
|----|--------|  
|[2.1 用戶端部署規劃](#21-plan-for-client-deployment)|規劃如何讓用戶端電腦使用 DirectAccess 進行連線。 決定哪些受管理的電腦將被設定為 DirectAccess 用戶端，以及計劃在用戶端電腦上部署「網路連線助理」或「DirectAccess 連線助理」。|  
|[2.2 為 DirectAccess 伺服器部署的的計劃](#22-plan-for-directaccess-server-deployment)|規劃如何部署 DirectAccess 伺服器。|  
|[2.3 規劃基礎結構伺服器](#23-plan-infrastructure-servers)|為您的 DirectAccess 部署規劃基礎結構伺服器，包括 DirectAccess 網路位置伺服器、「網域名稱系統」(DNS) 伺服器，以及 DirectAccess 管理伺服器。|  
|[2.4 規劃應用程式伺服器](#24-plan-application-servers)|為 IPv4 和 IPv6 應用程式伺服器做規劃，並視需要考量 DirectAccess 用戶端電腦與內部應用程式伺服器之間是否需要端對端驗證。|  
|[2.5 規劃 DirectAccess 和協力廠商 VPN 用戶端](#25-plan-directaccess-and-third-party-vpn-clients)|部署 DirectAccess 搭配協力廠商 VPN 用戶端時，可能必須設定登錄值，才能讓兩個遠端存取解決方案緊密並存。|  
  
## <a name="21-plan-for-client-deployment"></a>2.1 為用戶端部署做規劃  
規劃用戶端部署時，需要做三個決定：  
  
1.  DirectAccess 將僅供攜帶型電腦使用，或是可供任何電腦使用？  
  
    在「DirectAccess 用戶端安裝精靈」中設定 DirectAccess 用戶端時，您可以選擇只允許指定之安全性群組中的攜帶型電腦使用 DirectAccess 進行連線。 如果您限制只有攜帶型電腦才能存取，「遠端存取」就會自動設定 WMI 篩選器，以確保 DirectAccess 用戶端 GPO 只會套用到指定之安全性群組中的攜帶型電腦。 「遠端存取」系統管理員必須具備「建立」或「修改」安全性權限，才能建立或修改「群組原則物件」(GPO) WMI 篩選器來啟用這個設定。  
  
2.  哪些安全性群組將包含 DirectAccess 用戶端電腦？  
  
    DirectAccess 用戶端設定包含在 DirectAccess 用戶端 GPO 中。 這個 GPO 會套用到屬於您在「DirectAccess 用戶端安裝精靈」中指定之安全性群組的電腦。 您可以指定將安全性群組包含在任何支援的網域中。 如需詳細資訊，請參閱下一節[1.7 規劃 Active Directory 網域服務](da-adv-plan-s1-infrastructure.md#17-plan-active-directory-domain-services)。  
  
    設定 DirectAccess 之前，您應該先建立安全性群組。 您可以在完成 DirectAccess 部署之後將電腦新增到安全性群組，但是如果您新增的用戶端電腦位於與安全性群組不同的網域，用戶端 GPO 將不會套用到這些用戶端。 例如，如果您在網域 A 中為 DirectAccess 用戶端建立 SG1，並稍後從網域 B 將用戶端新增到這個群組，則用戶端 GPO 將不會套用到來自網域 B 的用戶端。為了避免這個問題，請為每個包含 DirectAccess 用戶端電腦的網域建立一個新的用戶端安全性群組。 或者，如果您不想要建立新的安全性群組，請使用新網域的新 GPO 名稱來執行 Windows PowerShell Cmdlet **Add-DAClient**。  
  
3.  您將為網路連線助理設定哪些設定？  
  
    網路連線助理會在用戶端電腦上執行，它會將有關 DirectAccess 連線的其他資訊提供給使用者。 在「DirectAccess 用戶端安裝精靈」中，您可以設定下列各項：  
  
    -   **連線能力檢查器**  
  
        系統會建立一個用戶端用來驗證內部網路連線能力的預設 Web 探查。 預設名稱為：  
  
        https://directaccess-WebProbeHost.<domain_name>  
  
        應該以手動方式在 DNS 中登錄這個名稱。 您可以透過 HTTP 使用其他網址或使用 **ping**，來建立其他連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。  
  
    -   **技術服務人員電子郵件地址**  
  
        如果使用者遇到 DirectAccess 連線問題，他們可以傳送包含診斷資訊的電子郵件給 DirectAccess 系統管理員，以進行問題疑難排解。  
  
    -   **DirectAccess 連線名稱**  
  
        指定一個 DirectAccess 連線名稱，以協助使用者識別其電腦上的 DirectAccess 連線。  
  
    -   **允許 DirectAccess 用戶端使用本機名稱解析**  
  
        用戶端需要一個在本機解析名稱的方式。 如果您允許 DirectAccess 用戶端使用本機名稱解析，使用者便可以使用本機 DNS 伺服器來解析名稱。 當使用者選擇使用本機 DNS 伺服器來進行名稱解析時，DirectAccess 就不會傳送單一標籤名稱的解析要求給公司內部 DNS 伺服器。 它會改用本機名稱解析 (藉由使用「連結-本機多點傳送名稱解析」(LLMNR) 和 NetBIOS over TCP/IP 通訊協定)。  
  
## <a name="22-plan-for-directaccess-server-deployment"></a>2.2 為 DirectAccess 伺服器部署做規劃  
當您計劃部署 DirectAccess 伺服器時，請考量下列決定：  
  
-   **網路拓撲**  
  
    部署 DirectAccess 伺服器時，有一些拓撲可用：  
  
    -   **兩張網路介面卡**。 搭配兩張網路介面卡時，可將 DirectAccess 設定為一張網路介面卡直接連線到網際網路，另一張連線到內部網路。 伺服器也可以安裝在邊緣裝置 (例如防火牆或路由器) 後面。 在此設定中，一張網路介面卡會連線到周邊網路，另一張則連線到內部網路。  
  
    -   **一張網路介面卡**。 在此設定中，DirectAccess 伺服器會安裝在邊緣裝置 (例如防火牆或路由器) 後面。 網路介面卡會連線到內部網路。  
  
    如需有關如何為您的部署選取拓撲的詳細資訊，請參閱 < [1.1 規劃網路拓樸和設定](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings)。  
  
-   **ConnectTo 位址**  
  
    用戶端電腦可以使用 ConnectTo 位址來連線到 DirectAccess 伺服器。 您選擇的位址必須與您為 IP-HTTPS  連線部署之 IP-HTTPS 憑證的主體名稱相符，並且必須是在公用 DNS 中可用的位址。  
  
-   **網路介面卡**  
  
    「遠端存取伺服器安裝精靈」會自動偵測 DirectAccess 伺服器上設定的網路介面卡。 您必須確定已選取正確的介面卡。  
  
-   **IP-HTTPS 憑證**  
  
    「遠端存取伺服器安裝精靈」會自動偵測適合 IP-HTTPS 連線的憑證。 您所選憑證的主體名稱必須與 ConnectTo 位址相符。 如果您使用自我簽署憑證，您可以選擇使用「遠端存取」伺服器自動建立的憑證。  
  
-   **IPv6 首碼**  
  
    如果「遠端存取伺服器安裝精靈」偵測到網路介面卡上已部署 IPv6，它就會自動填入內部網路的 IPv6 首碼、要指派給 DirectAccess 用戶端電腦的 IPv6 首碼，以及要指派給 VPN 用戶端電腦的 IPv6 首碼。 如果自動產生的首碼對您的原生 IPv6 基礎結構來說不正確，您就必須手動變更它們。 如需詳細資訊，請參閱 < [1.1 規劃網路拓樸和設定](da-adv-plan-s1-infrastructure.md#11-plan-network-topology-and-settings)。  
  
-   **驗證**  
  
    決定 DirectAccess 用戶端將如何向 DirectAccess 伺服器進行驗證：  
  
    -   **使用者驗證**。 您可以讓使用者使用 Active Directory 認證或雙因素驗證來進行驗證。 如需有關使用雙因素驗證進行驗證的詳細資訊，請參閱 <<c0> [ 使用 OTP 驗證部署遠端存取](https://technet.microsoft.com/library/hh831379.aspx)。  
  
    -   **電腦驗證**。 您可以設定讓電腦驗證使用憑證，或使用 DirectAccess 伺服器做為  Kerberos Proxy 來代表用戶端。 如需詳細資訊，請參閱 < [1.3 規劃憑證需求](da-adv-plan-s1-infrastructure.md#13-plan-certificate-requirements)。  
  
    -   **Windows 7 用戶端**。 根據預設，執行 Windows 7 的用戶端電腦無法連線到 Windows Server 2012 R2 或 Windows Server 2012 DirectAccess 部署。 如果您有貴組織中執行 Windows 7 的用戶端，而且它們需要遠端存取內部資源，您可以允許它們連線。 任何您想要允許存取內部資源的用戶端電腦都必須是您在「DirectAccess 用戶端安裝精靈」中指定之安全性群組的成員。  
  
        > [!NOTE]  
        > 允許執行透過 DirectAccess 連線的 Windows 7 的用戶端會要求您使用電腦憑證驗證。  
  
-   **VPN 設定**  
  
    設定 DirectAccess 之前，請先決定您是否要提供 VPN 存取給不支援 DirectAccess 的遠端用戶端。 如果您的組織中有不支援 DirectAccess 連線的用戶端電腦 (因為它們未受管理，或是執行不支援 DirectAccess 的作業系統) ，您就應該提供 VPN 存取。 「遠端存取伺服器安裝精靈」可讓您設定 IP 位址的指派方式 (藉由使用 DHCP 或從靜態位址集區)，以及 VPN 用戶端的驗證方式 (藉由使用 Active Directory 或「遠端驗證撥號服務」(RADIUS) 伺服器)。  
  
## <a name="23-plan-infrastructure-servers"></a>2.3 規劃基礎結構伺服器  
DirectAccess 需要三種類型的基礎結構伺服器：  
  
-   **DNS 伺服器**。 如需詳細資訊，請參閱 [1.4 規劃 DNS 需求](da-adv-plan-s1-infrastructure.md#14-plan-dns-requirements)  
  
-   **網路位置伺服器**。 如需詳細資訊，請參閱 [1.5 規劃網路位置伺服器](da-adv-plan-s1-infrastructure.md#15-plan-the-network-location-server)  
  
-   **管理伺服器**。 如需詳細資訊，請參閱 [1.6 規劃管理伺服器](da-adv-plan-s1-infrastructure.md#16-plan-management-servers)  
  
## <a name="24-plan-application-servers"></a>2.4 規劃應用程式伺服器  
應用程式伺服器是用戶端電腦可透過 DirectAccess 連線存取的公司網路上伺服器。 識別應用程式伺服器的方式是將它們新增到安全性群組中。 接著，應用程式伺服器 GPO 就會套用到該群組。  
  
> [!NOTE]  
> 只有當您要求端對端驗證和加密時，才需要將應用程式伺服器新增到安全性群組。  
  
您可以視需要要求在 DirectAccess 用戶端與所選應用程式內部伺服器之間進行端對端驗證和加密。 如果您設定端對端驗證，DirectAccess 用戶端就會使用 IPsec 傳輸原則。 這個原則會要求在指定的應用程式伺服器終止 IPsec 工作階段的驗證和流量保護。 在此情況下，「遠端存取」伺服器會將已驗證且受保護的 IPsec 工作階段轉送到應用程式伺服器。  
  
根據預設，當您將驗證延伸到應用程式伺服器時，DirectAccess 用戶端與應用程式伺服器之間的資料裝載會受到加密。 您可以選擇不將流量加密，而只使用驗證。 不過，這是安全性方面不如使用驗證和加密，而且它支援僅針對執行 Windows Server 2008 R2 或 Windows Server 2012 作業系統的應用程式伺服器。  
  
## <a name="25-plan-directaccess-and-third-party-vpn-clients"></a>2.5 規劃 DirectAccess 和協力廠商 VPN 用戶端  
有些協力廠商 VPN 用戶端不會在 [網路連線] 資料夾中建立連線。 這可能導致在已建立 VPN 連線且有內部網路連線能力時，DirectAccess 將它判斷為沒有內部網路連線能力。 當協力廠商 VPN 用戶端藉由將其介面定義為「網路裝置介面規格」(NDIS) 端點類型來登錄這些介面時，就會發生這種情況。 您可以藉由在 DirectAccess 用戶端上將下列登錄值設定為 1，讓這些類型的 VPN 用戶端並存：  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\ShowDomainEndpointInterfaces (REG_DWORD)**  
  
有些協力廠商 VPN 用戶端使用分割通道設定，此設定會允許 VPN 用戶端電腦直接存取網際網路，而不需要透過 VPN 連線將流量傳送到內部網路。  
  
分割通道設定通常會將 VPN 用戶端上的預設閘道設定保留為未設定或全部為零 (0.0.0.0)。 您可以藉由建立成功連到內部網路的 VPN 連線，並使用 Ipconfig.exe 命令列工具來檢視產生的設定，來加以確認。  
  
如果 VPN 連線列出的預設閘道為空白或全部為零 (0.0.0.0)，即表示您的 VPN 用戶端就是以這種方式設定的。 根據預設，DirectAccess 用戶端無法識別分割通道設定。 若要設定 DirectAccess 用戶端來偵測這些類型的 VPN 用戶端設定，並與其並存，請將下列登錄值設定為 1：  
  
**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\NlaSvc\Parameters\Internet\ EnableNoGatewayLocationDetection (REG_DWORD)**  
  
## <a name="previous-step"></a>上一步  
  
-   [步驟 1：規劃 DirectAccess 基礎結構](da-adv-plan-s1-infrastructure.md)  
  


