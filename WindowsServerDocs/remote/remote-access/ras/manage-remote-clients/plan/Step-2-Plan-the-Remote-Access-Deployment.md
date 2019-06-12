---
title: 步驟 2 規劃 「 遠端存取 」 部署
description: 本主題是指南 Windows Server 2016 中，從遠端管理 DirectAccess 用戶端的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc9f02b9-8ddd-4cae-b397-a832996144dd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7aec08a19759c98150cf7518643f634947c5133d
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805001"
---
# <a name="step-2-plan-the-remote-access-deployment"></a>步驟 2 規劃 「 遠端存取 」 部署

>適用於：Windows Server （半年通道），Windows Server 2016

您計劃您想要設定單一遠端存取伺服器的遠端管理 DirectAccess 用戶端使用的基礎結構之後，您就準備好規劃遠端存取安裝精靈將使用的設定。  
  
> [!NOTE]  
> 在繼續進行這些工作之前，請參閱[步驟 1:規劃遠端存取基礎結構](Step-1-Plan-the-Remote-Access-Infrastructure.md)。  
  
|工作|描述|  
|----|--------|  
|[規劃用戶端部署策略](#plan-a-client-deployment-strategy)|決定哪些受管理的電腦會設定為 DirectAccess 用戶端。|  
|[規劃遠端存取伺服器的部署策略](#plan-a-remote-access-server-deployment-strategy)|規劃如何部署遠端存取伺服器。|  
|[規劃基礎結構伺服器的組態](#plan-the-infrastructure-servers-configurations)|規劃遠端存取部署，包括 DirectAccess 網路位置伺服器、 DNS 伺服器和 DirectAccess 管理伺服器中的基礎結構伺服器。|  
  
## <a name="plan-a-client-deployment-strategy"></a>規劃用戶端部署策略  
規劃用戶端部署時，需要做三個決定：  
  
1.  將 DirectAccess 可攜帶型電腦，或指定的安全性群組中的每一部電腦嗎？  
  
    當您在 DirectAccess 用戶端安裝精靈設定 DirectAccess 用戶端時，您可以選擇只允許行動電腦在指定的安全性群組，來利用 DirectAccess 連接到伺服器。 如果您限制只有攜帶型電腦才能存取，「遠端存取」就會自動設定 WMI 篩選器，以確保 DirectAccess 用戶端 GPO 只會套用到指定之安全性群組中的攜帶型電腦。 遠端存取系統管理員需要建立或修改群組原則 WMI 篩選器的權限，才能啟用這個設定。  
  
2.  哪些安全性群組將包含 DirectAccess 用戶端電腦？  
  
    DirectAccess 設定包含 DirectAccess 用戶端群組原則物件 (GPO) 中。 這個 GPO 會套用到屬於您在「DirectAccess 用戶端安裝精靈」中指定之安全性群組的電腦。 您可以指定包含在任何支援的網域中的安全性群組。
  
    設定遠端存取之前，您需要建立的安全性群組。 在您完成 「 遠端存取 」 部署之後，可以將電腦新增到安全性群組。 不過，如果您新增用戶端電腦位於不同的網域安全性群組，用戶端 GPO 不會套用到這些用戶端。 例如，如果您在網域 A 中建立 SG1 的 DirectAccess 用戶端，您稍後將用戶端從 B 網域加入此群組，用戶端 GPO 將不會套用至網域 b 中的用戶端  
  
    若要避免這個問題，請建立新的 「 用戶端安全性 」 群組的每個包含用戶端電腦的網域。 或者，如果您不想要建立新的安全性群組，執行**Add-daclient** Windows PowerShell cmdlet 搭配新的網域的新 gpo 的名稱。  
  
3.  您會為 DirectAccess 網路連線助理設定哪些設定？  
  
    DirectAccess 網路連線助理會在用戶端電腦上執行，並提供 DirectAccess 連線的其他資訊給使用者。 在「DirectAccess 用戶端安裝精靈」中，您可以設定下列各項：  
  
    -   **連線能力檢查器**  
  
        系統會建立一個用戶端用來驗證內部網路連線能力的預設 Web 探查。 預設名稱是`https://directaccess-WebProbeHost.<domain_name>`。 應該以手動方式在 DNS 中登錄這個名稱。 您可以建立使用其他網址，透過 HTTP 或 PING 其他連線能力檢查器。 每個連線能力檢查器都必須有一個 DNS 項目。  
  
    -   **協助支援人員電子郵件地址**  
  
        如果使用者遇到 DirectAccess 連線問題，他們可以傳送電子郵件，其中包含的遠端存取管理員，可以針對問題進行疑難排解的診斷資訊。  
  
    -   **DirectAccess 連線名稱**  
  
        您可以指定要幫助使用者識別其電腦上的 DirectAccess 連線的 DirectAccess 連線名稱。  
  
    -   **允許 DirectAccess 用戶端使用本機名稱解析**  
  
        用戶端需要一種解析本機名稱。 如果您允許 DirectAccess 用戶端使用本機名稱解析，使用者便可以使用本機 DNS 伺服器來解析名稱。 當使用者選擇使用本機 DNS 伺服器進行名稱解析時，DirectAccess 不會傳送單一標籤名稱解析要求到內部公司 DNS 伺服器。 它改用本機名稱解析 （藉由使用連結-本機多點傳送名稱解析 (LLMNR) 和 NetBios over TCP/IP 通訊協定）。  
  
## <a name="plan-a-remote-access-server-deployment-strategy"></a>規劃遠端存取伺服器的部署策略  
您必須先在規劃部署遠端存取伺服器時的決策包括：  
  
-   **網路拓撲**  
  
    有兩個拓撲，部署遠端存取伺服器時：  
  
    -   **兩張介面卡**:有兩個網路介面卡，遠端存取可以設定一個網路介面卡直接連線到網際網路和其他連線到內部網路。 或，或者，伺服器會安裝在邊緣裝置，例如防火牆或路由器後面。 在此組態的一個網路介面卡連線到周邊網路，和其他連線到內部網路。  
  
    -   **單一網路介面卡**:在此組態中遠端存取伺服器會安裝在邊緣裝置，例如防火牆或路由器後面。 網路介面卡會連線到內部網路。  

-   **網路介面卡**  
  
    遠端存取伺服器安裝精靈 會自動偵測遠端存取伺服器設定的網路介面卡。 您必須確定已選取正確的介面卡。  
  
-   **IP-HTTPS 憑證**  
  
    「遠端存取伺服器安裝精靈」會自動偵測適合 IP-HTTPS 連線的憑證。 您所選憑證的主體名稱必須與 ConnectTo 位址相符。 如果您使用自我簽署的憑證，您可以選擇使用遠端存取伺服器自動建立的憑證。  
  
-   **IPv6 首碼**  
  
    如果「遠端存取伺服器安裝精靈」偵測到網路介面卡上已部署 IPv6，它就會自動填入內部網路的 IPv6 首碼、要指派給 DirectAccess 用戶端電腦的 IPv6 首碼，以及要指派給 VPN 用戶端電腦的 IPv6 首碼。 如果自動產生的首碼不適用您的原生 IPv6 或 ISATAP 基礎結構，您必須手動變更它們。  
  
-   **驗證**  
  
    您可以選擇其中一種下列方法，來驗證遠端存取伺服器的 DirectAccess 用戶端：  
  
    -   **使用者驗證**:您可以讓使用者使用 Active Directory 認證或雙因素驗證來進行驗證。  
  
    -   **電腦驗證**:您可以設定要使用憑證的電腦驗證。 或 「 遠端存取 」 伺服器時，可做為 Kerberos 驗證的 proxy 上，而不需要憑證。 
  
    -   **Windows 7 用戶端**根據預設，Windows 7 用戶端電腦無法連線到執行 Windows Server 2012 的遠端存取部署。 如果您有在組織中執行 Windows 7 用戶端需要遠端存取內部資源，您可以允許它們連線。 任何您想要允許存取內部資源的用戶端電腦都必須是您在「DirectAccess 用戶端安裝精靈」中指定之安全性群組的成員。  
  
        > [!NOTE]  
        > 允許執行透過 DirectAccess 連線的 Windows 7 的用戶端會要求您使用電腦憑證驗證。  
  
-   **VPN 設定**  
  
    設定遠端存取之前，決定是否您要提供 VPN 存取給遠端用戶端。 您應該提供 VPN 存取，如果您有貴組織中不支援 DirectAccess 連線的用戶端電腦 （比方說，它們未受管理或執行不支援 DirectAccess 的作業系統）。 遠端存取伺服器安裝精靈 可讓您設定 （藉由使用 DHCP 或靜態位址集區），如何指派 IP 位址和 VPN 用戶端驗證 （藉由使用 Active Directory 或 RADIUS 伺服器） 的方式。  
  
## <a name="plan-the-infrastructure-servers-configurations"></a>規劃基礎結構伺服器的組態  
遠端存取 」 需要三種類型的基礎結構伺服器：  
  
-   **網路位置伺服器**  
  
-   **DNS 伺服器** 
  
-   **管理伺服器** 
  
## <a name="see-also"></a>另請參閱  
  
-   [步驟 1：規劃遠端存取基礎架構](Step-1-Plan-the-Remote-Access-Infrastructure.md)  
  


