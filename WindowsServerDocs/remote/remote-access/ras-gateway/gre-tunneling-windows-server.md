---
title: Windows Server 2016 中的 GRE 通道
description: 您可以使用本主題以了解更新 Generic Routing Encapsulation (GRE) 通道功能的 Windows Server 2016 中的 RAS 閘道。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0ec077ad5e97edd3db7d1dc4e662bb191f7885b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887859"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Windows Server 2016 中的 GRE 通道

>適用於：Windows Server （半年通道），Windows Server 2016

Windows Server 2016 提供更新 Generic Routing Encapsulation \(GRE\) RAS 閘道的通道功能。  
  
GRE 是輕量級的通道通訊協定，可透過網際網路通訊協定網路封裝各種不同的虛擬點對點連結內的網路層通訊協定。 IPv4 和 IPv6，則可以將封裝 Microsoft GRE 實作。  
  
在許多案例中的 GRE 通道適合因為：  
  
-   它們是輕量型和 RFC 2890 相容，讓它與各種廠商裝置互通  
  
-   您可以使用邊界閘道協定\(BGP\)動態路由  
  
-   您可以使用軟體定義網路設定使用 GRE 多租用戶 RAS 閘道\(SDN\)
  
-   您可以使用 System Center Virtual Machine Manager 來管理 GRE\-型 RAS 閘道
  
-   您可以達到最多 2.0 Gbps 輸送量上 6 個核心的虛擬機器設定為 GRE RAS 閘道
  
-   單一閘道支援多個連接模式  
  
GRE 式通道可啟用租用戶虛擬網路與外部網路之間的連線。 因為 GRE 通訊協定是輕量型且支援 GRE 是適用於大部分的網路裝置變得的理想選擇通道加密資料不需要。 

站對站 (S2S) 通道中的 GRE 支援解決租用戶虛擬網路與使用多租用戶閘道的租用戶外部網路之間轉送的問題，如本主題稍後所述。  
  
GRE 通道功能被設計來滿足下列需求：  
  
-   主機服務提供者必須能夠建立虛擬網路的轉送，而不需要修改的實體交換器設定。  
  
-   主機服務提供者必須能夠加入其對外公開的網路中的子網路，而不需修改其基礎結構內的實體交換器的設定。  
GRE 通道功能啟用，或增強的主機服務提供者使用 Microsoft 技術來實作其服務供應項目中的 軟體定義網路的數個重要案例。  
  
以下是一些範例案例：  
  
-   [從租用戶的實體網路的租用戶虛擬網路的存取](#BKMK_Access)  
  
-   [高速連線](#BKMK_Speed)  
  
-   [VLAN 架構隔離與整合](#BKMK_Integration)  
  
-   [存取共用資源](#BKMK_Shared)  
  
-   [服務的租用戶的協力廠商裝置](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>主要案例

以下是 GRE 通道功能位址的重要案例。  
  
### <a name="BKMK_Access"></a>從租用戶的實體網路的租用戶虛擬網路的存取

此案例可讓您可調整的方式，以提供從租用戶上裝載的服務提供者端的實體網路的租用戶虛擬網路存取。 GRE 通道端點建立多租用戶閘道上，在實體網路上的協力廠商裝置上建立其他的 GRE 通道端點。 第 3 層流量會路由傳送虛擬網路中的虛擬機器和實體網路上的協力廠商裝置之間。  
  
![連線主機服務提供者的實體網路和租用戶虛擬網路的 GRE 通道](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="BKMK_Speed"></a>高速連線

此案例可讓您可調整的方式，以提供從租用戶內部部署網路到他們位於主機服務提供者網路的虛擬網路的高速連線。 租用戶會連線到透過多重通訊協定標籤交換 (MPLS)，主機服務提供者邊緣路由器與多租用戶的租用戶虛擬網路閘道之間建立 GRE 通道之後，服務提供者網路。  
  
![租用戶的企業 MPLS 網路和租用戶虛擬網路連接的 GRE 通道](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="BKMK_Integration"></a>VLAN 架構隔離與整合

此案例可讓您使用 HYPER-V 網路虛擬化整合 VLAN 架構隔離。 在 主控提供者網路上的實體網路包含負載平衡器使用 VLAN 架構隔離。 多租用戶閘道會建立負載平衡器上的實體網路與虛擬網路上的多租用戶閘道的 GRE 通道。  
  
可以在來源和目的地之間建立多個通道，並用來區分在通道之間 GRE 金鑰。  
  
![多個 GRE 通道連接的租用戶虛擬網路](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="BKMK_Shared"></a>存取共用資源

此案例可讓您存取位於主控提供者網路上的實體網路上的共用的資源。  
  
您可能必須位於您想要多個租用戶虛擬網路與共用主機提供者網路的實體網路上的伺服器上的共用的服務。  
  
具有非重疊的子網路的租用戶網路透過 GRE 通道存取常見的網路。 單一租用戶閘道會路由傳送 GRE 通道，因此路由到適當的租用戶網路的封包之間。  
  
在此案例中，可由協力廠商硬體設備取代單一租用戶閘道。  
  
![使用多個通道來連接多個虛擬網路的單一租用戶閘道](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="BKMK_thirdparty"></a>服務的租用戶的協力廠商裝置

此案例可以用來整合協力廠商裝置 （例如硬體負載平衡器），到租用戶虛擬網路流量。 比方說，從企業網站的流量通過 S2S 通道的多租用戶的閘道。 流量會路由至負載平衡器，透過 GRE 通道。 負載平衡器會將流量路由至企業的虛擬網路上的多部虛擬機器。 具有可能重疊的 IP 位址在虛擬網路中的另一個租用戶中發生相同的作業。 網路流量隔離使用 Vlan，請在負載平衡器，而且也適用於所有支援虛擬區域網路的第 3 層裝置。  
  
![多個 GRE 通道連接至協力廠商裝置的虛擬網路](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>設定和部署

GRE 通道會公開為 S2S 介面中的其他通訊協定。 它會以類似的方式中實作為下列網路功能部落格中所述的 IPSec S2S 通道：[使用 Windows Server 2012 R2 的多租用戶-網站 (S2S) VPN 閘道](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
請參閱下列主題的範例，會部署閘道，包括 GRE 通道閘道：  
  
[部署軟體定義網路基礎結構使用指令碼](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>詳細資訊

如需部署 S2S 閘道的詳細資訊，請參閱下列主題：  
  
-   [RAS 閘道](RAS-Gateway.md)  
  
-   [邊界閘道通訊協定&#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [新功能 ！Windows Server 2012 R2 RAS 多租用戶閘道部署指南](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [部署 RAS 多租用戶閘道的邊界閘道協定 (BGP)](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


