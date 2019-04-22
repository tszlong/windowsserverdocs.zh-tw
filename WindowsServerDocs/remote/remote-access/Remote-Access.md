---
title: 遠端存取
description: 本主題提供 Windows Server 2016 中的遠端存取伺服器角色的概觀。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/18/2018
ms.openlocfilehash: faf12ad22678fa58ea933613759e3e8414966bca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818359"
---
# <a name="remote-access"></a>遠端存取

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

遠端存取指南提供您在 Windows Server 2016 中，遠端存取伺服器角色的概觀，並涵蓋下列主題：

- [Alwayson VPN 部署指南](vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)
- [邊界閘道通訊協定&#40;BGP&#41;](bgp/Border-Gateway-Protocol-BGP.md)
- [RAS 閘道](ras-gateway/RAS-Gateway.md) 
- [遠端存取伺服器角色文件](ras/Remote-Access-Server-Role-Documentation.md)
- [適用於 SDN 的 RAS 閘道](../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
- [虛擬私人網路 (VPN)](vpn/vpn-top.md)
 
如需有關其他網路技術的詳細資訊，請參閱[Windows Server 2016 中的網路功能](https://docs.microsoft.com/windows-server/networking/networking)。

遠端存取伺服器角色是這些相關的網路存取技術的邏輯群組：[遠端存取服務 (RAS)](#bkmk_da)，[路由](#bkmk_rras)，以及[Web 應用程式 Proxy](#bkmk_proxy)。 這些技術是遠端存取伺服器角色的 *角色服務* 。 當您安裝遠端存取伺服器角色**新增角色及功能精靈**或 Windows PowerShell 中，您可以安裝一或多個這些三個角色服務。

>[!IMPORTANT]
>請勿嘗試在虛擬機器上部署遠端存取\(VM\)在 Microsoft Azure 中。 不支援在 Microsoft Azure 中使用遠端存取。 您無法使用 Azure VM 中的遠端存取部署 VPN、 DirectAccess 或 Windows Server 2016 或舊版的 Windows Server 中的任何其他遠端存取功能。 如需詳細資訊，請參閱 < [Microsoft Azure 虛擬機器的 Microsoft 伺服器軟體支援](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)。

## <a name="bkmk_da"></a>遠端存取服務\(RAS\) -RAS 閘道

當您安裝**DirectAccess 和 VPN (RAS)** 角色服務，您要部署 「 遠端存取服務通訊閘\( **RAS 閘道**\)。 您可以部署 RAS 閘道的單一租用戶 RAS 閘道虛擬私人網路\(VPN\)伺服器、 多租用戶的 RAS 閘道的 VPN 伺服器，並做為 DirectAccess 伺服器。

- **RAS 閘道-單一租用戶**。 藉由使用 RAS 閘道，您可以部署至使用者提供遠端存取您組織的網路和資源的 VPN 連線。 如果您的用戶端執行 Windows 10，您可以部署一律開啟 」 VPN，以維護用戶端與您的組織網路之間的持續連線，只要遠端電腦連線到網際網路。 透過 RAS 閘道，您也可以建立不同位置的兩部伺服器之間的站對站 VPN 連線這類之間主要辦公室及分公司辦公室，及使用網路位址轉譯\(NAT\)以便內的使用者網路可存取外部資源，例如網際網路。 此外，RAS 閘道支援邊界閘道通訊協定 (BGP)，可提供動態路由的服務，當遠端辦公室位置也有邊緣閘道支援 BGP。

- **RAS 閘道的多租用戶**。 當您使用 Hyper-v，您可以為多租用戶，以軟體為基礎的 edge 閘道和路由器部署 RAS 閘道\-網路虛擬化，或您已部署的虛擬區域網路與 VM 網路\(Vlan\)。 透過 RAS 閘道，雲端服務提供者\(Csp\)和企業可以啟用資料中心和雲端虛擬和實體網路，包括網際網路之間路由的網路流量。 RAS 閘道，與您的租用戶可以使用點讓站台 VPN 連線，從任何地方存取其資料中心內的 VM 網路資源。 您也可以將租用戶提供其遠端站台與 CSP 資料中心之間的站對站 VPN 連線中。 此外，您可以使用 BGP 動態路由，來設定 RAS 閘道，而且您可以啟用網路位址轉譯\(NAT\)來提供網際網路存取 vm，VM 網路上。

>[!IMPORTANT]
> RAS 閘道，透過多租用戶的功能也會提供 Windows Server 2012 R2 中的。

- **Alwayson VPN**。 一律開啟 」 VPN 可讓遠端使用者可以安全地存取共用的資源、 內部網路網站和內部網路上的應用程式，而不需要連線到 VPN。 

如需詳細資訊，請參閱 < [RAS 閘道](ras-gateway/RAS-Gateway.md)並[邊界閘道通訊協定 (BGP)](bgp/Border-Gateway-Protocol-BGP.md)。

## <a name="bkmk_rras"></a>路由

您可以使用遠端存取您的區域網路的子網路之間路由傳送網路流量。 路由網路位址轉譯 (NAT) 路由器執行 BGP、 路由資訊通訊協定 (RIP)，以及使用網際網路群組管理通訊協定 (IGMP) 支援多點傳送的路由器的 LAN 路由器提供支援。 為全功能的路由器中,，您可以部署 RAS 伺服器的電腦上，或為正在執行 HYPER-V 的電腦上的虛擬機器 (VM)。

若要安裝遠端存取的 LAN 路由器，請使用 [新增角色及功能精靈] 在 [伺服器管理員] 中，並選取**遠端存取**伺服器角色並**路由**角色服務或輸入下列命令命令在 Windows PowerShell 提示字元中，然後按 ENTER。

```  
Install-RemoteAccess -VpnType RoutingOnly
```  

## <a name="bkmk_proxy"></a>Web 應用程式 Proxy

Web Application Proxy 是 Windows Server 2016 中的遠端存取角色服務。 Web Application Proxy 為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以允許任何裝置上的使用者從公司網路外部存取這些應用程式。 Web 應用程式 Proxy 預先驗證存取 web 應用程式使用 Active Directory Federation Services (AD FS)，並也可做為 AD FS proxy。

若要安裝 Web 應用程式 Proxy 遠端存取，請使用 [新增角色及功能精靈] 在 [伺服器管理員] 中，並選取**遠端存取**伺服器角色並**Web 應用程式 Proxy**角色服務; 或在 Windows PowerShell 提示字元中，輸入下列命令，然後按 ENTER 鍵。  

```  
Install-RemoteAccess -VpnType SstpProxy  
```  

如需詳細資訊，請參閱 < [Web 應用程式 Proxy](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。


---