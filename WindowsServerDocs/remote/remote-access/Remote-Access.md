---
title: 遠端存取
description: 本主題提供 Windows Server 2016 中遠端存取服務器角色的總覽。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: d2fa9c82c4cab05b2a60916fee3f09c1ea48a472
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388914"
---
# <a name="remote-access"></a>遠端存取

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

《遠端存取指南》提供 Windows Server 2016 中的「遠端存取」伺服器角色的總覽，並涵蓋下列主題：

- [Always On VPN 部署指南](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [邊界閘道協定&#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [RAS 閘道](ras-gateway/RAS-Gateway.md) 
- [遠端存取伺服器角色文件](ras/Remote-Access-Server-Role-Documentation.md)
- [適用於 SDN 的 RAS 閘道](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [虛擬私人網路 (VPN)](vpn/vpn-top.md)
 
如需其他網路技術的詳細資訊，請參閱[Windows Server 2016 中的網路](https://docs.microsoft.com/windows-server/networking/networking)功能。

遠端存取服務器角色是這些相關網路存取技術的邏輯群組：[遠端存取服務（RAS）](#bkmk_da)、[路由](#bkmk_rras)和[Web 應用程式 Proxy](#bkmk_proxy)。 這些技術是遠端存取伺服器角色的 *角色服務* 。 當您使用 [**新增角色及功能] Wizard**或 Windows PowerShell 安裝遠端存取服務器角色時，您可以安裝這三個角色服務中的一或多個。

>[!IMPORTANT]
>請勿嘗試在 Microsoft Azure 的虛擬機器 \(VM\) 上部署遠端存取。 不支援在 Microsoft Azure 中使用遠端存取。 您無法在 Azure VM 中使用「遠端存取」來部署 VPN、DirectAccess，或 Windows Server 2016 或舊版 Windows Server 中的任何其他遠端存取功能。 如需詳細資訊，請參閱[Microsoft Microsoft Azure 虛擬機器的伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。

## <a name="bkmk_da"></a>遠端存取服務 \(RAS\)-RAS 閘道

當您安裝**DirectAccess 和 VPN （RAS）** 角色服務時，您會將遠端存取服務閘道部署 \(**RAS 閘道**\)。 您可以將 RAS 閘道部署為單一租使用者 RAS 閘道虛擬私人網路，\(VPN\) 伺服器、多租使用者 RAS 閘道 VPN 伺服器和 DirectAccess 伺服器。

- **RAS 閘道-單一租**使用者。 藉由使用 RAS 閘道，您可以部署 VPN 連線，讓使用者可以從遠端存取您組織的網路和資源。 如果您的用戶端執行的是 Windows 10，您可以部署 Always On VPN，這會在遠端電腦連線到網際網路時，維護用戶端與組織網路之間的持續連接。 使用 RAS 閘道時，您也可以在不同位置的兩部伺服器之間建立站對站 VPN 連線，例如主要辦公室和分公司，以及使用網路位址轉譯 \(NAT\) 讓網路內的使用者可以存取外部資源（例如網際網路）。 此外，當您的遠端辦公室位置也有支援 BGP 的邊緣閘道時，RAS 閘道支援邊界閘道協定（BGP），可提供動態路由服務。

- **RAS 閘道-** 多租使用者。 當您使用\-Hyper-v 網路虛擬化時，您可以將 RAS 閘道部署為多租使用者、以軟體為基礎的邊緣閘道和路由器，或者您的 VM 網路 \(Vlan\)部署了虛擬區域網路絡。 使用 RAS 閘道，\(Csp\) 的雲端服務提供者，企業可以在虛擬和實體網路（包括網際網路）之間啟用資料中心和雲端網路流量路由。 使用 RAS 閘道時，您的租使用者可以使用點對站 VPN 連線，從任何地方存取資料中心內的 VM 網路資源。 您也可以為租使用者提供其遠端網站與 CSP 資料中心之間的站對站 VPN 連線。 此外，您可以針對動態路由設定具有 BGP 的 RAS 閘道，也可以啟用網路位址轉譯 \(NAT\)，為 VM 網路上的 Vm 提供網際網路存取。

>[!IMPORTANT]
> Windows Server 2012 R2 也提供具有多租使用者功能的 RAS 閘道。

- **ALWAYS ON VPN**。 Always On VPN 可讓遠端使用者安全地存取內部網路上的共用資源、內部網站和應用程式，而不需要連線到 VPN。 

如需詳細資訊，請參閱[RAS 閘道](ras-gateway/RAS-Gateway.md)和[邊界閘道協定（BGP）](bgp/Border-Gateway-Protocol-BGP.md)。

## <a name="bkmk_rras"></a>傳入

您可以使用遠端存取，在區域網路上的子網之間路由傳送網路流量。 路由可支援使用網際網路群組管理通訊協定（IGMP）的網路位址轉譯（NAT）路由器、執行 BGP 的 LAN 路由器、路由資訊通訊協定（RIP）以及具備多播功能的路由器。 作為功能完整的路由器，您可以在伺服器電腦或執行 Hyper-v 之電腦上的虛擬機器（VM）部署 RAS。

若要將遠端存取安裝為 LAN 路由器，請使用伺服器管理員中的 [新增角色及功能]，然後選取 [**遠端存取**] 伺服器角色和 [**路由**角色服務]。或者，在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Web 應用程式 Proxy

Web 應用程式 Proxy 是 Windows Server 2016 中的遠端存取角色服務。 Web Application Proxy 為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以允許任何裝置上的使用者從公司網路外部存取這些應用程式。 Web 應用程式 Proxy 會使用 Active Directory 同盟服務（AD FS）預先驗證 web 應用程式的存取權，也可做為 AD FS Proxy。

若要將遠端存取安裝為 Web 應用程式 Proxy，請使用伺服器管理員中的 [新增角色及功能]，然後選取 [**遠端存取**] 伺服器角色和 [ **Web 應用程式 Proxy** ] 角色服務;或者，在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

如需詳細資訊，請參閱[Web 應用程式 Proxy](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。


---