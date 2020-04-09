---
title: RAS 閘道的新功能
description: 您可以使用本主題來瞭解 RAS 閘道的新功能，這是 Windows Server 2016 中以軟體為基礎、多租使用者、邊界閘道協定（BGP）且可支援的路由器。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 40d553ad5f41c68e8135c3d2c56ac8571d738e95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859581"
---
# <a name="whats-new-in-ras-gateway"></a>RAS 閘道的新功能

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解 RAS 閘道的新功能，這是 Windows Server 2016 中以軟體為基礎、多租使用者、邊界閘道協定（BGP）且可支援的路由器。 RAS 閘道多租使用者 BGP 路由器是針對雲端服務提供者（Csp）和使用 Hyper-v 網路虛擬化裝載多個租使用者虛擬網路的企業所設計。  
  
> [!NOTE]  
> 在 Windows Server 2012 R2 中，RAS 閘道名為 RRAS Gateway;而在 System Center Virtual Machine Manager 中，RAS 閘道的名稱是 Windows Server Gateway。  
  
本主題包含下列各節。  
  
-   [站對站連線能力選項](#bkmk_s2s)  
  
-   [閘道集區](#bkmk_pools)  
  
-   [閘道集區的擴充性](#bkmk_gps)  
  
-   [M + N 閘道集區冗余](#bkmk_m)  
  
-   [路由反射器](#bkmk_rr)  
  
## <a name="site-to-site-connectivity-options"></a><a name="bkmk_s2s"></a>站對站連線能力選項  
RAS 閘道現在支援三種類型的 VPN 站對站連線：網際網路金鑰交換版本2（IKEv2）站對站虛擬私人網路（VPN）、第3層（L3） VPN 和一般路由封裝（GRE）通道。  
  
如需 GRE 的詳細資訊，請參閱[Windows Server 2016 中的 Gre 通道](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="gateway-pools"></a><a name="bkmk_pools"></a>閘道集區  
在 Windows Server 2016 中，您可以建立不同類型的閘道集區。 閘道集區包含許多 RAS 閘道實例，並在實體和虛擬網路之間路由傳送網路流量。 閘道集區可以執行任何個別的閘道函式-網際網路金鑰交換版本2（IKEv2）站對站虛擬私人網路（VPN）、第3層（L3） VPN 和一般路由封裝（GRE）通道，或集區可以執行所有這些功能函式和作為混合集區。  
  
您可以根據您的基礎結構需求，使用您偏好的任何邏輯來建立閘道集區。 例如，您可以根據下列任何特性來建立閘道集區。  
  
-   通道類型（IKEv2 VPN、L3 VPN、GRE VPN）  
  
-   容量  
  
-   冗余層級（以您租使用者的計費方案為基礎的可靠性）  
  
-   客戶的自訂分隔  
  
如需詳細資訊，請參閱[RAS 閘道高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>閘道集區的擴充性  
您可以藉由在集區中新增或移除閘道 Vm，輕鬆地相應增加或減少閘道集區。 移除或新增閘道並不會中斷集區所提供的服務。 您也可以新增和移除整個閘道集區。  
  
如需詳細資訊，請參閱[RAS 閘道高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>M + N 閘道集區冗余  
每個閘道集區都是 M + N 多餘的。 這表示，' N ' 個作用中閘道虛擬機器（Vm）數目是由「N」個待命閘道 Vm 的數目所備份。 M + N 冗余可讓您更有彈性地決定部署 RAS 閘道時所需的可靠性層級。 您現在可以在每個作用中的 RAS 閘道 VM （這是 Windows Server 2012 R2 的唯一設定選項）中只使用一個待命 RAS 閘道，而不是在您所需的時間內設定多個待命 Vm。 如果作用中的 RAS 閘道 VM 失敗或失去連線能力，網路控制站閘道 Service Manager 功能會有效率地使用待命 RAS 閘道 VM 容量來提供可靠的容錯移轉。  
  
如需詳細資訊，請參閱[RAS 閘道高可用性](RAS-Gateway-High-Availability.md)。  
  
## <a name="route-reflector"></a><a name="bkmk_rr"></a>路由反射器  
邊界閘道協定（BGP）路由反映現在隨附于 RAS 閘道，並提供 BGP 完整網狀拓朴的替代方案，這是路由在路由器之間進行同步處理的必要項。 透過完整網狀同步處理，所有 BGP 路由器都必須與路由拓撲中的所有其他路由器連接。 不過，當您使用路由反映程式時，路由反映是唯一與所有其他路由器（稱為 BGP 用戶端）連接的路由器，因此可簡化路由同步處理並減少網路流量。 路由反映會學習所有路由、計算最佳路由，並將最佳路由轉散發至其 BGP 用戶端。  
  
使用 Windows Server 2016 時，您可以設定個別租使用者的遠端存取通道，以在多個 RAS 閘道 VM 上終止。 當有一個 RAS 閘道 VM 無法滿足租使用者連線的所有頻寬需求時，這可為雲端服務提供者提供更大的彈性。  
  
不過，這項功能引進了路由管理的額外複雜性，並有效地同步處理租使用者遠端網站與其在雲端資料中心內的虛擬資源之間的路由。 為租使用者提供多個 RAS 閘道的連線，也會在企業端增加設定的複雜性，其中每個租使用者網站都會有不同的路由鄰近專案。  
  
控制平面中的 BGP 路由反映會解決這些問題，並讓 CSP 內部網狀架構部署對企業租使用者而言是透明的。 以下是有關 RAS 閘道隨附並與網路控制站整合之 BGP 路由反映程式的一些重點。  
  
-   軟體定義網路部署中的路由反映程式是一種邏輯實體，位於 RAS 閘道與網路控制站之間的控制平面上。 不過，它不會參與資料平面路由。  
  
-   當您將新的租使用者新增至您的資料中心時，網路控制卡會自動將第一個租使用者 RAS 閘道設定為路由反射器。  
  
-   每個租使用者都有對應的路由反映程式，並位於與該租使用者相關聯的其中一個 RAS 閘道 Vm 上。  
  
-   租使用者路由反映程式會作為與租使用者相關聯的所有 RAS 閘道 Vm 的路由反映。 除了 RAS 閘道路由反射器以外的租使用者閘道是路由反映器用戶端。 路由反映程式會在所有路由反映程式用戶端之間執行路由同步處理，以進行實際的資料路徑路由。  
  
-   路由反映程式不會針對其設定所在的 RAS 閘道提供路由反射器服務。  
  
-   路由反映程式會使用對應至租使用者企業網站的企業路由來更新網路控制卡。 這可讓網路控制站在租使用者虛擬網路上設定所需的 Hyper-v 網路虛擬化原則，以進行端對端資料路徑存取。  
  
-   如果您的企業客戶使用客戶位址空間中的 BGP 路由，則 RAS 閘道路由反映程式是對應租使用者之所有網站的唯一外部 BGP （eBGP）鄰居。 無論企業租使用者的通道終止點為何，都是如此。 換句話說，無論 CSP 資料中心內的哪些 RAS 閘道 VM 終止租使用者網站的站對站 VPN 通道，所有租使用者網站的 eBGP 對等都是路由反射器。  
  
如需詳細資訊，請參閱[RAS 閘道部署架構](RAS-Gateway-Deployment-Architecture.md)和網際網路工程任務推動小組（IETF）要求批註主題[RFC 4456 BGP Route 反映：適用于全網格內部 BGP （IBGP）的替代方案](https://tools.ietf.org/html/rfc4456)。  
  

