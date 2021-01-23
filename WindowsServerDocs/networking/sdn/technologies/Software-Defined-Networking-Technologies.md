---
title: SDN 技術
description: 本節中的主題提供有關 Windows Server 2019 和2016中所含軟體定義網路技術的總覽和技術資訊。
manager: grcusanz
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: anpaul
author: AnirbanPaul
ms.date: 02/14/2019
ms.openlocfilehash: 62a0773fb51397cab2893826939736685eed798b
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716234"
---
# <a name="sdn-technologies"></a>SDN 技術

>適用於：Windows Server 2019、Windows Server 2016

本節中的主題提供有關 Windows Server 2019 和2016中所含軟體定義網路技術的總覽和技術資訊。

## <a name="network-controller"></a>[網路控制卡](network-controller/Network-Controller.md)

網路控制站提供集中、可程式化的自動化點，以管理、設定、監視和疑難排解資料中心內的虛擬和實體網路基礎結構。 您可以使用網路控制站，將網路基礎結構的設定自動化，而不需要手動設定網路裝置和服務。

網路控制站是高度可用且可擴充的伺服器，並提供兩個應用程式設計介面， (Api) ：

1. **SOUTHBOUND API** –可讓網路控制站與網路通訊。
2. **NORTHBOUND API** –可讓您與網路控制站通訊。

您可以使用 Windows PowerShell、具象狀態傳輸 (REST) API 或管理應用程式來管理下列實體和虛擬網路基礎結構：

- HYPER-V VM 和虛擬交換器
- 實體網路交換器
- 實體網路路由器
- 防火牆軟體
- VPN 閘道，包括遠端存取服務 (RAS) 多租使用者閘道
- 負載平衡器

## <a name="hyper-v-network-virtualization"></a>[Hyper-v 網路虛擬化](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Hyper-v 網路虛擬化 (HNV) 可協助您使用虛擬網路，從實體網路抽象化您的應用程式和工作負載。 虛擬網路會在共用的實體網路網狀架構上提供執行所需的多租用戶隔離，藉此提高資源使用率。 為了確保您可以繼續進行現有的投資，您可以在現有的網路設備上設定虛擬網路。 此外，虛擬網路與虛擬區域網路絡 (Vlan) 相容。

## <a name="hyper-v-virtual-switch"></a>[Hyper-V 虛擬交換器](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Hyper-v 虛擬交換器是以軟體為基礎的第2層乙太網路交換器，在您安裝 Hyper-v 伺服器角色之後，就可以在 Hyper-v 管理員中使用。 交換器包含以程式設計方式管理和可擴充的功能，將虛擬機器連線到虛擬網路和實體網路。 此外，Hyper-v 虛擬交換器提供安全性、隔離和服務層級的原則強制執行。

您也可以使用交換器內嵌小組來部署 Hyper-v 虛擬交換器 (設定) 和遠端直接記憶體存取 (RDMA) 。 如需詳細資訊，請參閱本主題中的 [遠端直接記憶體存取 (RDMA) 和切換內嵌團隊 (設定) ](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set) 。

## <a name="internal-dns-service-idns-for-sdn"></a>[SDN 的內部 DNS 服務 (iDNS) ](Idns-for-Sdn.md)

裝載的虛擬機器 (Vm) 和應用程式需要 DNS 在其網路內和網際網路上的外部資源進行通訊。 透過 iDNS，您可以為租使用者提供隔離的本機命名空間和網際網路資源的 DNS 名稱解析服務。

## <a name="network-function-virtualization"></a>[網路功能虛擬化](network-function-virtualization/Network-Function-Virtualization.md)

硬體設備（例如負載平衡器、防火牆、路由器和交換器）逐漸成為虛擬裝置。 Microsoft 有虛擬網路、交換器、閘道、Nat、負載平衡器和防火牆。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。 虛擬裝置會快速新興，並建立全新市場。 他們會持續產生興趣，並在虛擬化平臺和雲端服務中得到動力。

以下是可用的網路功能虛擬化技術。

-   **軟體 Load Balancer (SLB) ，以及 (NAT) 的網路位址轉譯**。 藉由支援直接伺服器回傳來增強輸送量，其中的傳回網路流量可以略過負載平衡多工器。 如需詳細資訊，請參閱 [SDN 的軟體負載平衡/ (SLB/) ](network-function-virtualization/software-load-balancing-for-sdn.md)。

-   **資料中心防火牆**。 提供 (Acl) 的細微存取控制清單，可讓您在 VM 介面層級或子網層級套用防火牆原則。 如需詳細資訊，請參閱 [資料中心防火牆總覽](network-function-virtualization/Datacenter-Firewall-Overview.md)。

-   **SDN 的 RAS 閘道**。 路由實體網路與 VM 網路資源之間的網路流量，而不論其位置為何。 您可以在相同的實體位置或許多不同的位置，路由傳送網路流量。 如需詳細資訊，請參閱 [SDN 的 RAS 閘道](network-function-virtualization/RAS-Gateway-for-SDN.md)。

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)
在 Windows Server 2016 中，您可以在系結至 Hyper-v 虛擬交換器的網路介面卡上啟用 RDMA （不論是否使用 Switch Embedded 組合 (設定) ）。 當您想要同時使用 RDMA 並設定時，這可讓您使用較少的網路介面卡。

SET 是替代的 NIC 小組解決方案，您可以在 Windows Server 2016 中包含 Hyper-v 和軟體定義網路 (SDN) 堆疊的環境中使用。 SET 可將部分 NIC 小組功能整合到 Hyper-v 虛擬交換器。

設定可讓您在一或多個實體 Ethernet 網路介面卡之間進行分組，以組成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。
設定成員網路介面卡必須全部安裝在相同的實體 Hyper-v 主機中，才能放置在小組中。

此外，您可以使用 Windows PowerShell 命令來啟用資料中心橋接 (DCB) 、建立具有 RDMA 虛擬 NIC (vNIC) 的 Hyper-v 虛擬交換器，以及建立具有設定和 RDMA Vnic 的 Hyper-v 虛擬交換器。 如需詳細資訊，請參閱 [ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

## <a name="border-gateway-protocol-bgp"></a>[邊界閘道通訊協定 (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)

邊界閘道協定 (BGP) 是動態路由通訊協定，它會自動學習使用站對站 VPN 連線的網站之間的路由。 因此，BGP 可減少手動設定路由器。   當您設定 RAS 閘道時，BGP 可讓您管理租使用者的 VM 網路與遠端網站之間的網路流量路由。

## <a name="software-load-balancing-slb-for-sdn"></a>[SDN 的軟體負載平衡 (SLB)](network-function-virtualization/software-load-balancing-for-sdn.md)
雲端服務提供者 (Csp) 和部署 SDN 的企業可以使用軟體負載平衡 (SLB) ，將租使用者和租使用者客戶網路流量平均分散到虛擬網路資源之間。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。

## <a name="windows-server-containers"></a>[Windows Server 容器](Containers/Container-networking-overview.md)

Windows Server 容器是輕量的作業系統虛擬化方法，可將應用程式或服務與相同容器主機上執行的其他服務分隔開來。 每個容器都有自己的作業系統、進程、檔案系統、登錄和 IP 位址，您可以連接到虛擬網路。

## <a name="system-center"></a>System Center

使用虛擬機器管理來部署和管理 SDN 基礎結構 [ (VMM) ](/system-center/vmm/) 和 [Operations Manager](/system-center/scom/)。 透過 VMM，您可以佈建及管理在私人雲端中建立及部署虛擬機器和服務所需的資源。  透過 Operations Manager，您可以監視整個企業中的服務、裝置和作業，進而找出問題並立即採取行動。


---