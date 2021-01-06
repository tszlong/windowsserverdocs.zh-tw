---
title: 網路函式虛擬化
description: 您可以使用本主題來瞭解網路功能虛擬化，此功能可讓您部署虛擬網路設備，例如資料中心防火牆、多租使用者 RAS 閘道，以及 Windows Server 2016 (SLB) 的軟體負載平衡。
manager: grcusanz
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: b5b2b4bb93a9d90e9c74f8310430ef3e7d826f1d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949084"
---
# <a name="network-function-virtualization"></a>網路函式虛擬化

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解網路功能虛擬化，此功能可讓您部署虛擬網路設備，例如資料中心防火牆、多租使用者 RAS 閘道和軟體負載平衡 SLB 多工器 \( \) \( MUX \) 。

>[!NOTE]
>除了本主題之外，下列網路功能虛擬化檔也可供使用。
> - [資料中心防火牆概觀](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)
> - [適用於 SDN 的 RAS 閘道](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
> - [SDN 的軟體負載平衡 (SLB)](./software-load-balancing-for-sdn.md)

在現今的軟體定義資料中心中，硬體 (設備所執行的網路功能（例如負載平衡器、防火牆、路由器、) 交換器等等）會逐漸虛擬化為虛擬裝置。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。 虛擬裝置會快速新興，並建立全新市場。 他們會持續產生興趣，並在虛擬化平臺和雲端服務中得到動力。

Microsoft 從 Windows Server 2012 R2 開始，包含獨立閘道作為虛擬應用裝置。 如需詳細資訊，請參閱 [Windows Server 閘道](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn313101(v=ws.11))。 現在有了 Windows Server 2016，Microsoft 會持續擴充並投資網路功能虛擬化市場。

## <a name="virtual-appliance-benefits"></a>虛擬裝置的優點
虛擬裝置是動態且容易變更的，因為它是預先建立的自訂虛擬機器。 它可以是一或多個以一個單位來封裝、更新和維護的虛擬機器。 您可以透過軟體定義的網路功能 (SDN) ，獲得現今雲端式基礎結構所需的靈活性和彈性。 例如：

-   SDN 以共用和動態資源的形式呈現網路。

-   SDN 有助於租使用者隔離。

-   SDN 最大化了規模和效能。

-   虛擬裝置可提供順暢的容量擴充和工作負載流動性。

-   虛擬裝置會將操作複雜度降至最低。

-   虛擬裝置可讓客戶輕鬆取得、部署及管理預先整合的解決方案。

    -   客戶可以輕鬆地將虛擬裝置移至雲端中的任何位置。

    -   客戶可以根據需求，動態地相應增加或相應減少虛擬裝置。

如需 Microsoft SDN 的詳細資訊，請參閱 [軟體定義的網路](../../software-defined-networking.md)功能。

### <a name="what-network-functions-are-being-virtualized"></a>虛擬化哪些網路功能？
虛擬化網路功能的 marketplace 正在快速成長。 以下是虛擬化的網路功能：

-   **安全性**

    -   防火牆

    -   防毒

    -   DDoS (分散式阻斷服務) 

    -   IPS/IDS (入侵防護系統/入侵偵測系統) 

-   **應用程式/WAN 優化工具**

-   **Edge**

    -   站對站閘道

    -   L3 閘道

    -   路由器

    -   交換器

    -   NAT

    -   負載平衡器 (不一定會在邊緣) 

    -   HTTP Proxy

## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>為什麼 Microsoft 是虛擬裝置的絕佳平臺
![虛擬網路堆疊](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)

Microsoft 平臺的設計是建立和部署虛擬裝置的絕佳平臺。 原因如下：

-   Microsoft 透過 Windows Server 2016 提供重要的虛擬化網路功能。

-   您可以從您選擇的廠商部署虛擬裝置。

-   您可以使用 Windows Server 2016 隨附的 Microsoft 網路控制器來部署、設定及管理您的虛擬裝置。 如需網路控制站的詳細資訊，請參閱 [網路控制](../../../sdn/technologies/network-controller/Network-Controller.md)站。

-   Hyper-v 可以裝載您需要的最上層客體作業系統。

## <a name="network-function-virtualization-in-windows-server-2016"></a>Windows Server 2016 中的網路功能虛擬化

### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Microsoft 提供的虛擬裝置功能
以下是 Windows Server 2016 隨附的虛擬裝置：

**軟體負載平衡器**

以資料中心規模運作的第4層負載平衡器。 這是在 Azure 環境中大規模部署的 Azure 負載平衡器的類似版本。 如需 Microsoft 軟體 Load Balancer 的詳細資訊，請參閱 [SDN 的軟體負載平衡 (SLB) ](/previous-versions/windows/server/mt632286(v=ws.12))。 如需 Microsoft Azure 負載平衡服務的詳細資訊，請參閱 [Microsoft Azure 負載平衡服務](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/)。

**閘道**。 RAS 閘道提供下列閘道功能的所有組合。

-   **站對站閘道**

    RAS 閘道提供邊界閘道協定 (支援 BGP) 的多租使用者閘道，可讓您的租使用者透過來自遠端網站的站對站 VPN 連線來存取和管理其資源，並允許雲端中的虛擬資源和租使用者實體網路之間的網路流量流動。 如需 RAS 閘道的詳細資訊，請參閱 [Ras 閘道高可用性](/previous-versions/windows/server/mt631692(v=ws.12)) 和 [ras 閘道](../../../../remote/remote-access/ras-gateway/ras-gateway.md)。

-   **轉送閘道**

    RAS 閘道會在虛擬網路與主機服務提供者實體網路之間路由傳送流量。 例如，如果租使用者建立一或多個虛擬網路，且需要在主機服務提供者存取實體網路上的共用資源，則轉送閘道可以在虛擬網路與實體網路之間路由傳送流量，讓使用者能夠使用其所需的服務來處理虛擬網路。 如需詳細資訊，請參閱 [Ras 閘道高可用性](/previous-versions/windows/server/mt631692(v=ws.12)) 和 [ras 閘道](../../../../remote/remote-access/ras-gateway/ras-gateway.md)。

-   **GRE 通道閘道**

    GRE 式通道可啟用租用戶虛擬網路與外部網路之間的連線。 由於 GRE 通訊協定是輕量的，且大部分的網路裝置都有支援 GRE，因此它會成為不需要資料加密的通道的理想選擇。 站對站 (S2S) 通道中的 GRE 支援使用多租用戶閘道解決了在租用戶虛擬網路與租用戶外部網路之間轉送的問題。 如需 GRE 通道的詳細資訊，請參閱 [Windows Server 2016 中的 Gre 通道](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。

**具有 BGP 的路由控制平面**

Hyper-v 網路虛擬化 (HNV) 路由控制是控制平面中的邏輯、集中式實體，其會攜帶所有客戶位址平面路由並動態學習，然後更新虛擬網路中的分散式 RAS 閘道路由器。 如需詳細資訊，請參閱 [Ras 閘道高可用性](/previous-versions/windows/server/mt631692(v=ws.12)) 和 [ras 閘道](../../../../remote/remote-access/ras-gateway/ras-gateway.md)。

**分散式多租使用者防火牆**

防火牆會保護虛擬網路的網路層。 原則會在每個租使用者 VM 的 SDN-vSwitch 埠上強制執行。 它會保護所有流量：東亞和北南部。 原則會透過租使用者入口網站推送，而網路控制站會將它們散發到所有適用的主機。 如需分散式多租使用者防火牆的詳細資訊，請參閱 [資料中心防火牆總覽](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。