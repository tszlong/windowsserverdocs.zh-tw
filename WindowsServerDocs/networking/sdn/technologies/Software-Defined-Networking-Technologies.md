---
title: SDN 技術
description: 在本節中的主題提供概觀和 Windows Server 2016 中所包含的軟體定義網路技術的技術資訊。
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: acf3e1dc3e5a229c525ba7cad23819640c0d5261
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891159"
---
# <a name="sdn-technologies"></a>SDN 技術

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

在本節中的主題提供概觀和 Windows Server 2016 中所包含的軟體定義網路技術的技術資訊。  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[網路控制站](network-controller/Network-Controller.md)

網路控制站提供集中式、 可程式化的自動化來管理、 設定、 監視和疑難排解虛擬和實體網路基礎結構，在您的資料中心點。 與網路控制站，您可以自動化網路基礎結構，而不是執行手動設定網路裝置和服務的組態。 

網路控制器是高度可用且可擴充的伺服器，並提供兩個應用程式開發介面 (Api):

1. **Southbound API** – 可讓網路控制站與網路通訊。
2. **Northbound API** – 可讓您與網路控制站進行通訊。

您可以使用 Windows PowerShell、 Representational State Transfer (REST) API 或管理應用程式來管理下列實體和虛擬網路基礎結構：

- HYPER-V VM 和虛擬交換器 
- 實體網路交換器 
- 實體網路路由器 
- 防火牆軟體 
- VPN 閘道，包括遠端存取服務 (RAS) 多租用戶閘道 
- 負載平衡器 
  

  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[HYPER-V 網路虛擬化](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

HYPER-V 網路虛擬化 (HNV) 可協助您使用虛擬網路抽象應用程式和工作負載從實體網路。 虛擬網路會在共用的實體網路網狀架構上提供執行所需的多租用戶隔離，藉此提高資源使用率。 若要確定，您可以盡快回收現有的投資，您可以設定現有的網路設備的虛擬網路。 此外，虛擬網路都與虛擬區域網路 (Vlan) 相容。   
  
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[HYPER-V 虛擬交換器](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

HYPER-V 虛擬交換器是軟體式 layer-2 乙太網路交換器，之後您已安裝 HYPER-V 伺服器角色是 HYPER-V Manager 中可用。 交換器包含以程式設計方式管理和可擴充的功能，將虛擬機器連線到虛擬網路和實體網路。 此外，HYPER-V 虛擬交換器提供安全性、 隔離以及服務等級的原則強化。
  
您也可以部署 HYPER-V 虛擬交換器，Switch Embedded Teaming (SET) 與遠端直接記憶體存取 (RDMA)。 如需詳細資訊，請參閱[遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](#bkmk_rdma)本主題中。  

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[SDN 的內部的 DNS 服務 (iDNS)](Idns-for-Sdn.md)

裝載的虛擬機器 (Vm) 和應用程式需要 DNS，將其網路內，以及與網際網路上的外部資源進行通訊。 使用的 iDNS，您可以將租用戶提供隔離、 本機命名空間和網際網路資源的 DNS 名稱解析服務。 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[網路功能虛擬化](network-function-virtualization/Network-Function-Virtualization.md)

硬體設備，例如負載平衡器、 防火牆、 路由器和交換器越來越容易成為虛擬設備。 Microsoft 有虛擬網路、 交換器、 閘道、 Nat、 負載平衡器，以及防火牆。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。 虛擬設備是快速的新興和建立全新的市場。 它們繼續產生感興趣並取得趨勢電子報，在這兩個虛擬化平台上部署和雲端服務。 
  
使用下列網路功能虛擬化技術。  
  
-   **軟體負載平衡器 (SLB) 和網路位址轉譯 (NAT)**。 支援 Direct Server Return 在其中傳回的網路流量可以略過負載平衡多工器，以提升輸送量。 如需詳細資訊，請參閱 <<c0> [ 適用於 SDN 的軟體負載平衡 /(SLB/)](network-function-virtualization/software-load-balancing-for-sdn.md)。
  
-   **資料中心防火牆**。 提供更細微的存取控制清單 (Acl)，讓您套用 VM 介面層級或子網路層級的防火牆原則。 如需詳細資訊，請參閱 <<c0> [ 資料中心防火牆概觀](network-function-virtualization/Datacenter-Firewall-Overview.md)。
  
-   **適用於 SDN 的 RAS 閘道**。 實體網路和 VM 網路資源，不論位置之間的路由傳送網路流量。 您可以路由傳送網路流量，在相同的實體位置或許多不同的位置。 如需詳細資訊，請參閱 <<c0> [ 適用於 SDN 的 RAS 閘道](network-function-virtualization/RAS-Gateway-for-SDN.md)。

  
## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-sethttpsdocsmicrosoftcomwindows-servervirtualizationhyper-v-virtual-switchrdma-and-switch-embedded-teaming"></a>[遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)  
在 Windows Server 2016 中，您可以繫結到使用或不含 Switch Embedded Teaming (SET) 的 HYPER-V 虛擬交換器的網路介面卡上啟用 RDMA。 這可讓您使用較少的網路介面卡，當您想要使用 RDMA 和設定在相同的時間。  
  
集合是替代的 NIC 小組解決方案，您可以使用包含 Windows Server 2016 中的 HYPER-V 和軟體定義網路 (SDN) 堆疊的環境中。 設定整合的部分 NIC 小組功能到 HYPER-V 虛擬交換器。  
  
設定可讓您一到八個實體乙太網路介面卡之間到一或多個以軟體為基礎的虛擬網路介面卡群組。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。  
集合成員的網路介面卡必須全部安裝在相同的實體 HYPER-V 主機來放置在一個小組。  
  
此外，您可以使用 Windows PowerShell 命令來啟用資料中心橋接 (DCB)、 建立 HYPER-V 虛擬交換器使用 RDMA 虛擬 NIC (vNIC)，並使用 SET 和 RDMA Vnic 建立 HYPER-V 虛擬交換器。  

  

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[邊界閘道協定 (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
邊界閘道通訊協定 (BGP) 是自動學習使用站對站 VPN 連線的站台之間路由的動態路由通訊協定。 因此，BGP 可以降低手動設定路由器。   當您設定 RAS 閘道時，BGP 可讓您管理的租用戶 VM 網路與遠端站台之間的網路流量路由。  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[軟體負載平衡 (SLB)，適用於 SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
雲端服務提供者 (Csp) 和企業部署 SDN，可以使用軟體負載平衡 (SLB) 將租用戶和租用戶客戶網路流量在虛擬網路資源之間平均地分散。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Windows Server 容器](Containers/Container-networking-overview.md)

Windows Server 容器是輕量型作業系統虛擬化方法，將相同的容器主機上執行的其他服務應用程式或服務。 每個容器都有自己的作業系統、 程序、 檔案系統、 登錄和 IP 位址，您可以連接到虛擬網路。 


## <a name="system-center"></a>System Center  
部署和管理 SDN 基礎結構與[虛擬機器管理 (VMM)](https://docs.microsoft.com/system-center/vmm/)並[Operations Manager](https://docs.microsoft.com/system-center/scom/)。 使用 VMM，您可以佈建和管理建立和部署到私人雲端的虛擬機器和服務所需的資源。  使用 Operations Manager，您以找出問題立即採取行動企業監視服務、 裝置和作業。 


---