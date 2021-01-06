---
title: Windows Server 支援的網路功能案例
description: 本主題提供 Windows Server 2016 和更新版本中新支援的網路案例的相關資訊
manager: brianlic
ms.topic: article
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
author: dcuomo
ms.author: dacuo
ms.date: 08/07/2020
ms.openlocfilehash: b8d6b0c3f983ca4798569b06f0a08c95e82106d4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949304"
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server 支援的網路功能案例

>適用于： Windows Server \( 半年通道 \) 、windows server 2016

本主題提供在此 Windows Server 2016 版本中，您可以或無法執行的支援和不支援案例的相關資訊。
>[!IMPORTANT]
>針對所有的生產案例，請使用來自原始設備製造商 \( OEM \) 或獨立硬體廠商 IHV 的最新已簽署硬體驅動程式 \( \) 。

## <a name="supported-networking-scenarios"></a><a name="bkmk_supp"></a>支援的網路案例

本節包含 Windows Server 2016 支援的網路案例的相關資訊，並包含下列案例類別。

-   [軟體定義的網路 (SDN) 案例](#bkmk_sdn)

-   [網路平臺案例](#bkmk_netp)

-   [DNS 伺服器案例](#bkmk_dns)

-   [使用 DHCP 和 DNS 的 IPAM 案例](#bkmk_ipam)

-   [NIC 小組案例](#bkmk_nicteam)

- [切換內嵌的團隊 \( 設定 \) 案例](#bkmk_set)

### <a name="software-defined-networking-sdn-scenarios"></a><a name="bkmk_sdn"></a>軟體定義的網路 (SDN) 案例

您可以使用下列檔，以 Windows Server 2016 部署 SDN 案例。


-   [使用指令碼部署軟體定義網路的基礎結構](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)

如需詳細資訊，請參閱 [軟體定義網路 &#40;SDN&#41;](sdn/software-defined-networking.md)。

#### <a name="network-controller-scenarios"></a><a name="bkmk_netc"></a>網路控制站案例

網路控制站案例可讓您：

-   部署和管理多節點的網路控制站實例。 如需詳細資訊，請參閱 [使用 Windows PowerShell 部署網路控制](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)站。

-   使用網路控制站，以程式設計方式使用 REST Northbound API 定義網路原則。

-   使用 NVGRE 或 VXLAN 封裝，以使用網路控制站來建立和管理使用 Hyper-v 網路虛擬化的虛擬網路。

如需詳細資訊，請參閱[網路控制卡](sdn/technologies/network-controller/Network-Controller.md)。

#### <a name="network-function-virtualization-nfv-scenarios"></a><a name="bkmk_netf"></a>網路功能虛擬化 (NFV) 案例
NFV 案例可讓您：

-   部署和使用軟體負載平衡器，以分散 northbound 和 southbound 流量。

-   部署和使用軟體負載平衡器，以針對使用 Hyper-v 網路虛擬化建立的虛擬網路，散發 eastbound 和 westbound 流量。

-   針對使用 Hyper-v 網路虛擬化建立的虛擬網路，部署和使用 NAT 軟體負載平衡器。

-   部署和使用第3層轉送閘道

-   針對站對站 IPsec (IKEv2) 通道，部署和使用虛擬私人網路 (VPN) 閘道

-   部署和使用一般路由封裝 (GRE) 閘道。

-   使用邊界閘道協定 (BGP) ，在網站之間部署及設定動態路由和傳輸路由。

-   針對第3層和站對站閘道以及 BGP 路由設定 M + N 冗余。

-   使用網路控制卡指定虛擬網路和網路介面上的 Acl。

如需詳細資訊，請參閱 [網路功能虛擬化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。

### <a name="network-platform-scenarios"></a><a name="bkmk_netp"></a>網路平臺案例

在本節的案例中，Windows Server 網路團隊支援使用任何 Windows Server 2016 認證的驅動程式。 請洽詢您的網路介面卡 \( NIC \) 製造商，確定您有最新的驅動程式更新。

網路平臺案例可讓您：

-   使用交集的 NIC，以使用單一網路介面卡結合 RDMA 和 Ethernet 流量。

-   使用封包直接存取、在 Hyper-v 虛擬交換器中啟用，以及單一網路介面卡來建立低延遲的資料路徑。

-   將設定為可將 SMB Direct 和 RDMA 流量分散到最多兩個網路介面卡。

如需詳細資訊，請參閱 [&#40;RDMA 的遠端直接記憶體存取&#41; 和切換內嵌小組 &#40;設定&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

#### <a name="hyper-v-virtual-switch-scenarios"></a><a name="bkmk_switch"></a>Hyper-v 虛擬交換器案例

Hyper-v 虛擬交換器案例可讓您：

-   使用遠端直接記憶體存取 (RDMA) vNIC 建立 Hyper-v 虛擬交換器

-   使用交換器內嵌團隊 (設定) 和 RDMA Vnic 來建立 Hyper-v 虛擬交換器

-   在 Hyper-v 虛擬交換器中建立集合小組

-   使用 Windows PowerShell 命令來管理集合小組

如需詳細資訊，請參閱 [&#40;RDMA 的遠端直接記憶體存取&#41; 和切換內嵌小組 &#40;設定&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)

### <a name="dns-server-scenarios"></a><a name="bkmk_dns"></a>DNS 伺服器案例

DNS 伺服器案例可讓您：

-   使用 DNS 原則指定以 Geo-Location 為基礎的流量管理

-   使用 DNS 原則設定分割腦 DNS

-   使用 DNS 原則對 DNS 查詢套用篩選

-   使用 DNS 原則設定應用程式負載平衡

-   根據一天中的時間指定智慧型 DNS 回應

-   設定 DNS 區域轉移原則

-   在 Active Directory Domain Services (AD DS) 整合區域上設定 DNS 伺服器原則

-   設定回應速率限制

-    (來) 指定名為實體的 DNS 型驗證

-   設定 DNS 中未知記錄的支援

如需詳細資訊，請參閱 [Windows server 2016 中 Dns 用戶端的新功能](dns/What-s-New-in-DNS-Client.md) ，以及 [windows SERVER 2016 中 Dns 伺服器的新功能](dns/What-s-New-in-DNS-Server.md)。

### <a name="ipam-scenarios-with-dhcp-and-dns"></a><a name="bkmk_ipam"></a>使用 DHCP 和 DNS 的 IPAM 案例

IPAM 案例可讓您：

-   探索及管理跨多個同盟 Active Directory 樹系的 DNS 和 DHCP 伺服器和 IP 位址

-   使用 IPAM 集中管理 DNS 屬性，包括區域和資源記錄。

-   定義細微的角色型存取控制原則，並委派 IPAM 使用者或使用者群組來管理您指定的一組 DNS 屬性。

-   使用 IPAM 的 Windows PowerShell 命令將 DHCP 和 DNS 的存取控制設定自動化。

    如需詳細資訊，請參閱 [管理 IPAM](technologies/ipam/Manage-IPAM.md)。

### <a name="nic-teaming-scenarios"></a><a name="bkmk_nicteam"></a>NIC 小組案例

NIC 小組案例可讓您：

-   在支援的設定中建立 NIC 小組

-   刪除 NIC 小組

-   在支援的設定中，將網路介面卡新增至 NIC 小組

-   從 NIC 小組移除網路介面卡

> [!NOTE]
> 在 Windows Server 2016 中，您可以使用 Hyper-v 中的 NIC 小組，但在某些情況下，當您建立 NIC 小組時，) 可能不會在基礎網路介面卡上自動啟用 (VMQ 的虛擬機器佇列。 如果發生這種情況，您可以使用下列 Windows PowerShell 命令，以確保已在 NIC 小組成員介面卡上啟用 VMQ： `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`

如需詳細資訊，請參閱 [NIC](technologies/nic-teaming/NIC-Teaming.md)小組。

### <a name="switch-embedded-teaming-set-scenarios"></a><a name="bkmk_set"></a>切換內嵌的團隊 \( 設定 \) 案例

SET 是替代的 NIC 小組解決方案，您可以在 Windows Server 2016 中包含 Hyper-v 和軟體定義網路 (SDN) 堆疊的環境中使用。 SET 可將某些 NIC 小組功能整合到 Hyper-v 虛擬交換器。

如需詳細資訊，請參閱 [ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)



## <a name="unsupported-networking-scenarios"></a><a name="bkmk_unsupp"></a>不支援的網路案例
Windows Server 2016 不支援下列網路功能案例。

-   以 VLAN 為基礎的租使用者虛擬網路。

-   基礎或重迭都不支援 IPv6。