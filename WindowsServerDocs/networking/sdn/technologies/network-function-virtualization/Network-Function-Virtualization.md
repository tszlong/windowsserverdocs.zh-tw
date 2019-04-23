---
title: 網路函式虛擬化
description: 若要了解網路功能虛擬化，可讓您部署虛擬網路設備，例如資料中心防火牆、 多租用戶 RAS 閘道和軟體負載平衡 (SLB) Windows Server 2016 中，您可以使用本主題。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59474a13d1cbce6a607f025caf3f6c1b839c7eed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884549"
---
# <a name="network-function-virtualization"></a>網路函式虛擬化

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來深入了解網路功能虛擬化，可讓您部署虛擬網路的設備，例如資料中心防火牆、 多租用戶 RAS 閘道，以及軟體負載平衡\(SLB\)多工器\(MUX\)。
  
>[!NOTE]  
>本主題中，除了下列網路功能虛擬化文件使用。  
> - [資料中心防火牆概觀](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [適用於 SDN 的 RAS 閘道](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [軟體負載平衡 (SLB)，適用於 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
在現今的軟體會逐漸所定義的資料中心，要由硬體設備 （例如負載平衡器、 防火牆、 路由器、 交換器等等） 的網路功能虛擬化為虛擬設備。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。 虛擬設備是快速的新興和建立全新的市場。 它們繼續產生感興趣並取得趨勢電子報，在這兩個虛擬化平台上部署和雲端服務。  
  
Microsoft 會包含獨立閘道，為從 Windows Server 2012 R2 的虛擬設備。 如需詳細資訊，請參閱 [Windows Server 閘道](https://technet.microsoft.com/library/dn313101.aspx)。 現在與 Windows Server 2016 的 Microsoft 會繼續展開，然後將心力灌注在網路函式虛擬化市場。  
  
## <a name="virtual-appliance-benefits"></a>虛擬設備的優點  
虛擬設備是動態且易於變更，因為它是預先建置的自訂虛擬機器。 它可以是一或多個虛擬機器封裝、 更新和維護當做一個單位。 搭配使用軟體定義網路 (SDN)，您取得靈活度及現今的雲端式基礎結構中所需的彈性。 例如:   
  
-   SDN 呈現為集區和動態資源的網路。  
  
-   SDN 可協助租用戶隔離。  
  
-   SDN 發揮最大的規模和效能。  
  
-   虛擬設備啟用無縫式的容量擴充和工作負載行動性。  
  
-   虛擬設備降低操作複雜度。  
  
-   虛擬設備，可讓客戶輕鬆地取得、 部署及管理預先整合的解決方案。  
  
    -   客戶可以輕鬆地移動的虛擬應用裝置隨處在雲端中。  
  
    -   客戶可以相應增加虛擬設備，或向下動態地根據需求。  
  
如需有關 Microsoft SDN[軟體定義網路](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-)。  
  
### <a name="what-network-functions-are-being-virtualized"></a>哪些網路函式會被虛擬化？  
Marketplace 中的虛擬化的網路功能快速成長。 下列網路功能會被虛擬化：  
  
-   **安全性**  
  
    -   防火牆  
  
    -   防毒  
  
    -   DDoS （分散式阻斷服務）  
  
    -   IPS/IDS （入侵預防系統/入侵偵測系統）  
  
-   **應用程式/WAN 最佳化程式**  
  
-   **Edge**  
  
    -   站對站閘道  
  
    -   L3 閘道  
  
    -   路由器  
  
    -   參數  
  
    -   NAT  
  
    -   負載平衡器 （不一定是在邊緣）  
  
    -   HTTP proxy  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Microsoft 為何虛擬設備的絕佳平台  
![虛擬網路堆疊](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
Microsoft 平台已工程化，是絕佳的平台，建置及部署虛擬應用裝置。 原因如下：  
  
-   Microsoft 提供 Windows Server 2016 的重要的虛擬化的網路函式。  
  
-   您可以部署您所選擇的虛擬設備廠商。  
  
-   您可以部署、 設定及管理您與 Microsoft 網路控制卡隨附於 Windows Server 2016 的虛擬設備。 如需有關網路控制站的詳細資訊，請參閱[網路控制卡](../../../sdn/technologies/network-controller/Network-Controller.md)。  
  
-   HYPER-V 可以裝載您需要的最上層的客體作業系統。  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Windows Server 2016 中的網路功能虛擬化  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>由 Microsoft 所提供的虛擬設備函式  
Windows Server 2016 提供下列虛擬設備：  
  
**軟體負載平衡器**  
  
在資料中心大規模運作的第 4 層負載平衡器。 這是已在 Azure 環境中部署 Azure 的負載平衡器類似版本。 如需有關 Microsoft 軟體負載平衡器的詳細資訊，請參閱[軟體負載平衡 (SLB) 適用於 SDN](https://technet.microsoft.com/library/mt632286.aspx)。 如需有關 Microsoft Azure 負載平衡服務的詳細資訊，請參閱 < [Microsoft Azure 負載平衡服務](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/)。  
  
**閘道**。 RAS 閘道提供下列閘道函式的所有組合。  
  
-   **站對站閘道**  
  
    RAS 閘道提供邊界閘道通訊協定 (BGP)-功能、 多租用戶閘道，可讓您的租用戶來存取和管理其資源，透過站對站 VPN 連線，從遠端站台，並可讓虛擬資源之間的網路流量流程雲端和租用戶實體網路中。 如需 RAS 閘道的詳細資訊，請參閱[RAS 閘道高可用性](https://technet.microsoft.com/library/mt631692.aspx)並[RAS 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **轉送閘道**  
  
    RAS 閘道路由傳送虛擬網路與主機的提供者實體網路之間的流量。 例如，如果租用戶建立一個或多個虛擬網路，而且需要在主機服務提供者的實體網路上的共用資源的存取權，轉送閘道可以路由傳送虛擬網路與實體網路，以提供使用者在工作之間的流量含有所需的服務的虛擬網路。 如需詳細資訊，請參閱 < [RAS 閘道高可用性](https://technet.microsoft.com/library/mt631692.aspx)並[RAS 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **GRE 通道閘道**  
  
    GRE 式通道可啟用租用戶虛擬網路與外部網路之間的連線。 因為 GRE 通訊協定是輕量型且支援 GRE 是大部分的網路裝置上使用，它會變成的理想選擇通道，不需要資料加密。 站對站 (S2S) 通道中的 GRE 支援使用多租用戶閘道解決了在租用戶虛擬網路與租用戶外部網路之間轉送的問題。 如需 GRE 通道的詳細資訊，請參閱[Windows Server 2016 中的 GRE 通道](https://technet.microsoft.com/library/dn765485.aspx)。  
  
**Bgp 路由的控制平面**  
  
HYPER-V 網路虛擬化 (HNV) 路由的控制項是在控制平面，其帶有所有客戶地址平面路由，且會以動態方式學習，然後更新虛擬網路中的分散式的 RAS 閘道路由器的邏輯、 集中式的實體。 如需詳細資訊，請參閱 < [RAS 閘道高可用性](https://technet.microsoft.com/library/mt631692.aspx)並[RAS 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
**分散式的多租用戶防火牆**  
  
防火牆保護虛擬網路的網路的層。 在每個租用戶 VM 的 SDN vSwitch 連接埠，會強制執行原則。 它能保護所有流量的流動： 東-西和北-南。 原則推送到租用戶入口網站，網路控制站將它們發佈至所有適用的主機。 如需有關分散式的多租用戶防火牆的詳細資訊，請參閱 <<c0> [ 資料中心防火牆概觀](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  


