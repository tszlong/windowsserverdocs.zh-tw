---
title: 管理遠端存取
description: 本主題提供如何在 Windows Server 2016 中管理遠端存取的相關資訊。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 1459819a-b1b6-4800-8770-4a85d02c7a2b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8379818d22d51fe8c4cc983f32d017d3cdee52b6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965580"
---
# <a name="manage-remote-access"></a>管理遠端存取

>適用於：Windows Server (半年度管道)、Windows Server 2016

「DirectAccess 遠端用戶端管理」部署案例會使用 DirectAccess 來維護透過網際網路的用戶端。 本節說明案例，包括其階段、角色、功能及其他資源的連結。  
  
Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與路由及遠端存取服務（RRAS） VPN 合併成一個遠端存取角色。   
  
> [!NOTE]  
> 除了本主題之外，還有下列遠端存取管理的主題可以使用。  
>   
> -   [使用遠端存取監視和計量](monitoring-and-accounting/Use-Remote-Access-Monitoring-and-Accounting.md)  
> -   [遠端管理 DirectAccess 用戶端](manage-remote-clients/Manage-DirectAccess-Clients-Remotely.md)  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>案例描述  
每當 DirectAccess 用戶端電腦連線到網際網路時，不論使用者是否登入電腦，都會連線到內部網路。 它們可以當做內部網路資源管理，並隨時保持與群組原則變更、作業系統更新、反惡意程式碼更新與其他組織變更同步更新。  
  
在某些情況下，內部網路伺服器或電腦必須主動連接 DirectAccess 用戶端。 例如，技術支援部門技術人員可以使用遠端桌面連線，連線至遠端 DirectAccess 用戶端並且進行疑難排解。 這個案例可以讓您將現有的存取方案應用到使用者網路連接，讓 DirectAccess 只用於遠端管理。  
  
DirectAccess 提供支援遠端系統管理 DirectAccess 用戶端的設定。 您可以使用部署精靈選項，接受一種特別限管的設定，也就是只有需要從遠端管理用戶端電腦的時候，才可以建立原則。  
  
> [!NOTE]  
> 在這種部署中，使用者等級組態選項，例如強制通道、網路存取保護 (NAP) 整合，以及雙因素驗證，都將無法使用。  
  
## <a name="in-this-scenario"></a>在這個案例中  
「DirectAccess 遠端用戶端管理」部署案例包括用於計劃和設定的下列步驟：  
  
### <a name="plan-the-deployment"></a>計劃部署  
計劃本案例時，只有幾項電腦和網路需求： 其中包括：  
  
-   **網路和伺服器拓撲**：有了 DirectAccess 之後，您可以將遠端存取伺服器放到內部網路的邊緣，或者放到網路位址轉譯 (NAT) 裝置或防火牆的後面。  
  
-   **DirectAccess 網路位置伺服器**：DirectAccess 用戶端會使用網路位置伺服器來判斷它們是否位於內部網路。 網路位置伺服器可以安裝到 DirectAccess 伺服器或其他伺服器。  
  
-   **DirectAccess 用戶端**：決定哪些受管理的電腦會設定為 DirectAccess 用戶端。  
  
### <a name="configure-the-deployment"></a>設定部署  
設定部署包含一些步驟。 其中包含：  
  
1.  **設定基礎結構**：設定 DNS 設定值、必要時將伺服器和用戶端電腦加入網域，以及設定 Active Directory 安全性群組。  
  
    在此部署案例中，遠端存取會自動建立群組原則物件 (GPO)。 如需 advanced certificate GPO 選項，請參閱[部署 Advanced Remote Access](assetId:///3475e527-541f-4a34-b940-18d481ac59f6)。  
  
2.  **設定遠端存取伺服器和網路設定**：設定網路介面卡、IP 位址和路由。  
  
3.  **設定憑證設定**：在此部署案例中，消費者入門 Wizard 會建立自我簽署憑證，因此不需要設定更先進的憑證基礎結構。  
  
4.  **設定網路位置伺服器**：在這個案例中，網路位置伺服器會安裝到遠端存取伺服器。  
  
5.  **計劃 DirectAccess 管理伺服器**：系統管理員可以使用網際網路，從遠端管理位於公司網路外部的 DirectAccess 用戶端電腦。 管理伺服器包括遠端用戶端管理 (例如更新伺服器) 期間使用的電腦。  
  
6.  **設定遠端存取伺服器**：安裝遠端存取角色以及執行 DirectAccess 開始使用精靈來設定 DirectAccess。  
  
7.  **確認部署**：測試用戶端，確定它可以利用 DirectAccess 連接內部網路以及網際網路。  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用  
部署單一的遠端存取伺服器，用於管理 DirectAccess 用戶端時，可以提供以下功能：  
  
-   **易於存取**：執行 Windows 8 或 Windows 7 的受管理用戶端電腦可以設定為 DirectAccess 用戶端電腦。 這些用戶端只要連線到網際網路便可隨時透過 DirectAccess 存取內部網路資源，而不需登入 VPN 連線。 未執行上述作業系統的用戶端電腦可以透過 VPN 連接內部網路。 DirectAccess 和 VPN 都在同一個主控台，以及使用相同的一組精靈進行管理。  
  
-   **容易管理**：遠端存取系統管理員可以透過 DirectAccess，從遠端管理連線到網際網路的 DirectAccess 用戶端電腦，即使用戶端電腦不位於公司內部網路上也可以。 管理伺服器可以自動修復不符合公司規定的用戶端電腦。 只要一部遠端存取管理主控台就可以管理一或多個遠端存取伺服器。  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>這個案例包含的角色與功能  
下表列出本案例必備的角色和功能：  
  
|角色或功能|如何支援本案例|  
|----------|-----------------|  
|*遠端存取角色*|使用伺服器管理員主控台或 Windows PowerShell 安裝和解除安裝這個角色。 這個角色包含 DirectAccess (以前是 Windows Server 2008 R2 的功能) 與路由及遠端存取服務 (以前是網路原則與存取服務 (NPAS) 伺服器角色底下的角色服務)。 遠端存取角色包含兩個元件：<p>1. DirectAccess 與路由及遠端存取服務（RRAS） VPN： DirectAccess 和 VPN 是在遠端存取管理主控台中進行管理。<br />2. RRAS：功能會在 [路由及遠端存取] 主控台中進行管理。<p>遠端存取伺服器角色需要下列功能：<p>-Web Server （IIS）：設定網路位置伺服器和預設 Web 探查的必要動作。<br />-Windows 內部資料庫：用於遠端存取服務器上的本機帳戶處理。|  
|遠端存取管理工具功能|這個功能的安裝方式如下：<p>-安裝遠端存取角色時，預設在遠端存取服務器上，並支援遠端管理主控台使用者介面。<br />-做為未執行遠端存取服務器角色之伺服器上的選項。 在此情況下，它是用於遠端管理遠端存取伺服器。<p>這項功能包含下列項目：<p>-遠端存取 GUI 和命令列工具<br />-適用于 Windows PowerShell 的遠端存取模組<p>依存項目包括：<p>-群組原則管理主控台<br />-RAS 連線管理員系統管理元件（CMAK）<br />-Windows PowerShell 3。0<br />-圖形化管理工具與基礎結構|  
  
## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>硬體需求  
本案例需要的硬體如下所示：  
  
### <a name="server-requirements"></a>伺服器需求  
  
-   符合 Windows Server 2016 硬體需求的電腦。 如需詳細資訊，請參閱 Windows Server 2016[系統需求](../directaccess/prerequisites-for-deploying-directaccess.md)。  
  
-   伺服器必須至少安裝以及啟用一張網路介面卡。 應該只有一張介面卡連接公司內部網路，而且只有一張介面卡連接外部網路 (網際網路)。  
  
-   如果 Teredo 必須當作 IPv6 與 IPv4 轉換通訊協定，則伺服器的外部介面卡需要兩個連續的公用 IPv4 位址。 如果只有一張網路介面卡，則只能使用 IP-HTTPS 做為轉換通訊協定。  
  
-   至少一個網域控制站。 遠端存取伺服器和 DirectAccess 用戶端都必須是網域成員。  
  
-   如果您不想要將自我簽署憑證用於 IP-HTTPS 或網路位置伺服器，或是想要使用用戶端憑證來進行用戶端 IPsec 驗證，則伺服器上需要憑證授權單位。  
  
### <a name="client-requirements"></a>用戶端需求  
  
-   用戶端電腦必須執行 Windows 10 或 Windows 8 或 Windows 7。  
  
### <a name="infrastructure-and-management-server-requirements"></a>基礎結構和管理伺服器需求  
  
-   從遠端管理 DirectAccess 用戶端電腦時，用戶端會與管理伺服器進行通訊，例如網域控制站、系統中心設定伺服器以及健康情況登錄授權單位 (HRA) 伺服器。 這些伺服器會提供服務，包含 Windows 和防毒更新以及網路存取保護 (NAP) 用戶端規範。 開始部署遠端存取之前，您應該先部署必要的伺服器。  
  
-   需要執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 含 SP2 的 DNS 伺服器。  
  
## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>軟體需求  
本案例的軟體需求如下：  
  
### <a name="server-requirements"></a>伺服器需求  
  
-   遠端存取伺服器必須是網域成員。 伺服器可以部署到內部網路的邊緣，或者在邊緣防火牆或其他裝置的後面。  
  
-   如果遠端存取伺服器是位在邊緣防火牆或 NAT 裝置後面，則裝置必須設定成允許遠端存取伺服器的連入和連出通訊。  
  
-   負責部署遠端存取的系統管理員，必須具備伺服器的本機系統管理員身分，並且擁有網域使用者權限。 此外，系統管理員需要具備部署 DirectAccess 所使用之 GPO 的權限。 如果想利用這些功能，將 DirectAccess 只部署到行動電腦上，必須具備在網域控制站建立 WMI 篩選器的網域系統管理員權限。  
  
-   如果網路位置伺服器不是位在遠端存取伺服器上，則需要一台獨立的伺服器來執行網路位置伺服器。  
  
### <a name="remote-access-client-requirements"></a>遠端存取用戶端需求  
  
-   DirectAccess 用戶端必須是網域成員。 包含用戶端的網域可以和遠端存取伺服器屬於相同的樹系，或者與遠端存取伺服器樹系或網域存在雙向信任的關係。  
  
-   準備一個 Active Directory 安全性群組，只要會設定成 DirectAccess 用戶端的電腦，全部放到這個群組中。 電腦不可以包含在多個內含 DirectAccess 用戶端的安全性群組中。 如果用戶端是包含在多個群組中，則無法正常為每一個用戶端要求，進行名稱的解析工作。  
  
