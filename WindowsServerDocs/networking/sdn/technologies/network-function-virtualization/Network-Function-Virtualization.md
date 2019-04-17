---
title: 網路功能模擬
description: 您可以使用本主題以深入了解網路功能模擬，可讓您部署 virtual 網路的裝置，例如 Datacenter 防火牆、multitenant RAS 閘道和軟體負載平衡 (SLB) 在 Windows Server 2016。
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
ms.openlocfilehash: 7caa9eef42cb7ab95a13d64c1dcd3639b1132eb3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-function-virtualization"></a>網路功能模擬

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以深入了解網路功能模擬，可讓您部署 virtual 網路的裝置，例如 Datacenter 防火牆 multitenant RAS 閘道，以及軟體負載平衡 \(SLB\) 多工器 \(MUX\)。
  
>[!NOTE]  
>本主題中，除了下列網路功能模擬文件會提供。  
> - [Datacenter 防火牆概觀](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [適用於 SDN RAS 閘道](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [軟體負載平衡 SDN (SLB)](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
在今天的軟體定義資料中心硬體裝置（例如負載平衡器、防火牆、路由器、參數，等等）來執行網路功能的越來越正在擬化檔案為 virtual 裝置。 這「網路功能模擬」是伺服器模擬和網路模擬自然進展。 快速新興，建立的全新市場 virtual 裝置。 他們繼續產生興趣取得待發這兩個模擬平台和雲端服務。  
  
Microsoft 以開始使用 Windows Server 2012 R2 的 virtual 應用裝置隨附的獨立閘道。 如需詳細資訊，請查看[Windows 伺服器閘道](https://technet.microsoft.com/library/dn313101.aspx)。 現在與 Windows Server 2016 Microsoft 會繼續以展開及投資網路功能模擬市場。  
  
## <a name="virtual-appliance-benefits"></a>Virtual 應用裝置權益  
Virtual 應用裝置，而且動態輕鬆變更，因為它是預先建置，自訂一樣。 它可以是一或多個虛擬電腦已封裝、更新，並為單位保留。 一起軟體定義網路 (SDN)，就會顯示靈活度和今天以雲端為基礎的基礎結構中所需的彈性。 例如：  
  
-   SDN 呈現網路為集區與動態資源。  
  
-   幫助您承租人隔離 SDN。  
  
-   SDN 發揮最大的縮放比例和效能。  
  
-   可讓順暢容量擴充和工作負載行動 virtual 裝置。  
  
-   Virtual 設備最小化操作複雜。  
  
-   Virtual 設備可讓您輕鬆地取得、部署及管理預先整合的方案針對。  
  
    -   針對可以輕鬆地移動 virtual 應用裝置任何位置點一下在雲端中。  
  
    -   針對可縮放 virtual 設備或向下動態根據要求。  
  
如需有關 Microsoft SDN 查看[軟體定義網路](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-)。  
  
### <a name="what-network-functions-are-being-virtualized"></a>網路功能的問題擬化檔案？  
快速變大的市場模擬的網路功能。 下列網路功能的問題擬化檔案：  
  
-   **安全性**  
  
    -   防火牆  
  
    -   防毒軟體  
  
    -   DDoS（分散式阻斷服務）  
  
    -   IPS ID（入侵防止系統日入侵偵測系統）  
  
-   **應用程式日 WAN 最佳化**  
  
-   **Edge**  
  
    -   網站-閘道  
  
    -   L3 閘道  
  
    -   路由器  
  
    -   切換  
  
    -   NAT  
  
    -   負載平衡器（不一定是在 edge 中)  
  
    -   HTTP proxy  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>為何 Microsoft 會 virtual 裝置變得更好的平台  
![網路 virtual 堆疊](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
Microsoft 平台經過工程設計為建置及部署 virtual 裝置變得更好的平台。 原因如下：  
  
-   Microsoft 提供與 Windows Server 2016 金鑰模擬的網路功能。  
  
-   您可以部署 virtual 應用廠商裝置的您的選擇。  
  
-   您可以部署、設定及管理您使用的是 Windows Server 2016 的 Microsoft Network Controller 的 virtual 裝置。 如需 Network Controller 的詳細資訊，請查看[Network Controller](../../../sdn/technologies/network-controller/Network-Controller.md)。  
  
-   HYPER-V 可以主機頂端來賓作業系統，您需要。  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Windows Server 2016 中的網路功能模擬  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Microsoft 所提供的 virtual 設備函式  
提供下列 virtual 裝置與 Windows Server 2016:  
  
**軟體負載平衡器**  
  
層級 4 負載平衡器在 datacenter 縮放作業。 這是在縮放 Azure 環境中部署的 Azure 負載平衡器版本類似。 如需 Microsoft 軟體負載平衡器，請查看[軟體負載平衡 (SLB) SDN 的](https://technet.microsoft.com/library/mt632286.aspx)。 如需 Microsoft Azure 負載平衡服務的詳細資訊，請查看[Microsoft Azure 負載平衡服務](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/)。  
  
**閘道**。 RAS 閘道提供所有下列閘道功能的組合。  
  
-   **網站-閘道**  
  
    RAS 閘道提供邊境閘道通訊協定 (BGP)-可讓您存取及管理他們的資源從遠端網站，以網站 VPN 連接到 tenants 並允許 virtual 資源中的雲端和承租人實體網路間網路流量的功能、multitenant 閘道。 如需 RAS 閘道的詳細資訊，請查看[RAS 閘道可用性](https://technet.microsoft.com/library/mt631692.aspx)和[RAS 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **轉送閘道**  
  
    RAS 閘道傳送 virtual 網路與控管提供者實體網路間流量。 例如，如果 tenants 建立一或多個 virtual 網路，必須存取實體網路裝載的提供者共用資源轉接閘道可以傳送 virtual 網路與提供使用者使用 virtual 服務所需的網路上的實體網路間流量。 如需詳細資訊，請查看[RAS 閘道可用性](https://technet.microsoft.com/library/mt631692.aspx)和[RAS 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **GRE 通道閘道**  
  
    GRE 根據承租人 virtual 網路之間外部網路的通道讓連接。 因為 GRE 通訊協定輕量型與支援 GRE 是網路的大部分裝置上，它會變成的資料加密不需要理想選擇的通道。 支援網站 (S2S) 通道 GRE 下轉接承租人 virtual 網路與承租人外部網路使用多承租人閘道之間的 如需 GRE 可愛的詳細資訊，請查看[在 Windows Server 2016 的 GRE 通道](https://technet.microsoft.com/library/dn765485.aspx)。  
  
**使用 BGP 路由控制平面**  
  
HYPER-V 網路模擬 (HNV) 路由控制項是控制平面，帶來客戶地址平面路徑和動態學習，然後更新分散式的 RAS 閘道路由器 virtual 網路中的邏輯，打造實體。 如需詳細資訊，請查看[RAS 閘道可用性](https://technet.microsoft.com/library/mt631692.aspx)和[RAS 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
**散發多承租人防火牆**  
  
防火牆保護網路 virtual 網路層的級。 原則會執行的每個承租人 VM SDN-vSwitch 連接埠。 保護所有流量：東西和北南。 原則會透過承租人入口網站推入和 Network Controller 它們分散至所有適用的主機。 如需分散式多承租人防火牆的詳細資訊，請查看[Datacenter 防火牆概觀](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  


