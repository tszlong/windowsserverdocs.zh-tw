---
title: 網路函式虛擬化
description: 您可以使用本主題來瞭解網路功能虛擬化，這可讓您部署虛擬網路設備（例如資料中心防火牆、多租使用者 RAS 閘道，以及 Windows Server 2016 中的軟體負載平衡（SLB））。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1d5d5aaae5983e062dae203c60a7001f36e5629b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309812"
---
# <a name="network-function-virtualization"></a>網路函式虛擬化

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解網路功能虛擬化，這可讓您部署虛擬網路設備（例如資料中心防火牆、多租使用者 RAS 閘道，以及軟體負載平衡） \(SLB\) 多工器 \(MUX\)。
  
>[!NOTE]  
>除了本主題之外，也提供下列網路功能虛擬化檔。  
> - [資料中心防火牆概觀](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [適用於 SDN 的 RAS 閘道](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [SDN 的軟體負載平衡 (SLB)](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
在現今的軟體定義資料中心，硬體設備（例如負載平衡器、防火牆、路由器、交換器等）所執行的網路功能，會逐漸虛擬化成為虛擬應用裝置。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。 虛擬裝置很快就會推出，並建立全新的市場。 他們會繼續在虛擬化平臺和雲端服務中產生興趣並取得動力。  
  
Microsoft 從 Windows Server 2012 R2 開始，將獨立閘道納入為虛擬裝置。 如需詳細資訊，請參閱 [Windows Server 閘道](https://technet.microsoft.com/library/dn313101.aspx)。 現在，有了 Windows Server 2016 之後，Microsoft 就會繼續擴充並投資網路功能虛擬化市場。  
  
## <a name="virtual-appliance-benefits"></a>虛擬裝置的優點  
虛擬裝置是動態且易於變更的，因為它是預先建立的自訂虛擬機器。 它可以是一或多個以一個單位封裝、更新及維護的虛擬機器。 搭配軟體定義網路（SDN），您可以在現今的雲端式基礎結構中取得所需的靈活性和彈性。 例如，  
  
-   SDN 會將網路呈現為集區和動態資源。  
  
-   SDN 有助於租使用者隔離。  
  
-   SDN 最大化了規模和效能。  
  
-   虛擬裝置可實現順暢的容量擴充和工作負載流動性。  
  
-   虛擬裝置可將操作複雜度降至最低。  
  
-   虛擬裝置可讓客戶輕鬆取得、部署和管理預先整合的解決方案。  
  
    -   客戶可以輕鬆地將虛擬裝置移到雲端的任何地方。  
  
    -   客戶可以根據需求動態地相應增加或相應減少虛擬裝置。  
  
如需 Microsoft SDN 的詳細資訊，請參閱[軟體定義的網路](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-)功能。  
  
### <a name="what-network-functions-are-being-virtualized"></a>虛擬化哪些網路功能？  
虛擬化網路功能的 marketplace 會快速成長。 已虛擬化下列網路功能：  
  
-   **安全性**  
  
    -   防火牆  
  
    -   防毒軟體  
  
    -   DDoS （分散式阻斷服務）  
  
    -   IP/識別碼（入侵預防系統/入侵偵測系統）  
  
-   **應用程式/WAN 優化工具**  
  
-   **下邊**  
  
    -   站對站閘道  
  
    -   L3 閘道  
  
    -   路由器  
  
    -   交換機  
  
    -   NAT  
  
    -   負載平衡器（不一定在邊緣）  
  
    -   HTTP proxy  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>為什麼 Microsoft 是虛擬裝置的絕佳平臺  
![虛擬網路堆疊](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
Microsoft 平臺已設計成絕佳的平臺，可建立和部署虛擬裝置。 原因如下：  
  
-   Microsoft 提供 Windows Server 2016 的主要虛擬化網路功能。  
  
-   您可以從您選擇的廠商來部署虛擬裝置。  
  
-   您可以使用 Windows Server 2016 隨附的 Microsoft 網路控制站來部署、設定和管理虛擬裝置。 如需網路控制卡的詳細資訊，請參閱[網路控制](../../../sdn/technologies/network-controller/Network-Controller.md)卡。  
  
-   Hyper-v 可以裝載您所需的熱門客體作業系統。  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Windows Server 2016 中的網路功能虛擬化  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Microsoft 所提供的虛擬裝置功能  
Windows Server 2016 提供下列虛擬裝置：  
  
**軟體負載平衡器**  
  
以資料中心規模運作的第4層負載平衡器。 這是在 Azure 環境中大規模部署的類似 Azure 負載平衡器版本。 如需 Microsoft 軟體 Load Balancer 的詳細資訊，請參閱[適用于 SDN 的軟體負載平衡（SLB）](https://technet.microsoft.com/library/mt632286.aspx)。 如需 Microsoft Azure 負載平衡服務的詳細資訊，請參閱[Microsoft Azure 負載平衡服務](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/)。  
  
**閘道**。 RAS 閘道提供下列閘道功能的所有組合。  
  
-   **站對站閘道**  
  
    RAS 閘道提供支援邊界閘道協定（BGP）的多租使用者閘道，可讓您的租使用者透過遠端網站的站對站 VPN 連線來存取和管理其資源，並允許虛擬資源之間的網路流量流動在雲端和租使用者實體網路中。 如需 RAS 閘道的詳細資訊，請參閱[Ras 閘道高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[ras 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **轉送閘道**  
  
    RAS 閘道會在虛擬網路與主機服務提供者實體網路之間路由傳送流量。 例如，如果租使用者建立一或多個虛擬網路，而且需要存取主機服務提供者上實體網路上的共用資源，則轉送閘道可以在虛擬網路與實體網路之間路由傳送流量，以提供使用者工作具有所需服務的虛擬網路。 如需詳細資訊，請參閱[Ras 閘道高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[ras 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
-   **GRE 通道閘道**  
  
    GRE 式通道可啟用租用戶虛擬網路與外部網路之間的連線。 因為 GRE 通訊協定是輕量的，而且在大部分的網路裝置上都可支援 GRE，所以它會成為不需要資料加密的通道的理想選擇。 站對站 (S2S) 通道中的 GRE 支援使用多租用戶閘道解決了在租用戶虛擬網路與租用戶外部網路之間轉送的問題。 如需 GRE 通道的詳細資訊，請參閱[Windows Server 2016 中的 Gre 通道](https://technet.microsoft.com/library/dn765485.aspx)。  
  
**具有 BGP 的路由控制平面**  
  
Hyper-v 網路虛擬化（HNV）路由控制是控制平面中的邏輯、集中式實體，它會攜帶所有客戶位址平面路由，並以動態方式學習並更新虛擬網路中的分散式 RAS 閘道路由器。 如需詳細資訊，請參閱[Ras 閘道高可用性](https://technet.microsoft.com/library/mt631692.aspx)和[ras 閘道](https://technet.microsoft.com/library/mt626650.aspx)。  
  
**分散式多租使用者防火牆**  
  
防火牆會保護虛擬網路的網路層。 原則會在每個租使用者 VM 的 SDN vSwitch 埠上強制執行。 它會保護所有流量：美國西部和北南部。 原則會透過租使用者入口網站進行推送，而網路控制站會將它們散發到所有適用的主機。 如需分散式多租使用者防火牆的詳細資訊，請參閱[資料中心防火牆總覽](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  


