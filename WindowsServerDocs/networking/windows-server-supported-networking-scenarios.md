---
title: Windows Server 支援的網路功能案例
description: 本主題提供 Windows Server 2016 和更新版本中新支援之網路案例的相關資訊
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f338ddf0a7d3a4fe41277ddbf49b0c3db34ae11b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395704"
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server 支援的網路功能案例

>適用於：Windows Server \(Semi-每年通道 @ no__t-1、Windows Server 2016

本主題提供您可以或無法在此 Windows Server 2016 版本中執行之支援和不支援案例的相關資訊。  
>[!IMPORTANT]
>針對所有生產案例，請使用原始設備製造商所提供的最新簽署硬體驅動程式 \(OEM @ no__t-1 或獨立硬體廠商 \(IHV @ no__t-3。
  
## <a name="bkmk_supp"></a>支援的網路案例

本節包含 Windows Server 2016 支援的網路功能案例的相關資訊，並包含下列案例分類。  
  
-   [軟體定義網路（SDN）案例](#bkmk_sdn)  
  
-   [網路平臺案例](#bkmk_netp)  
  
-   [DNS 伺服器案例](#bkmk_dns)  
  
-   [使用 DHCP 和 DNS 的 IPAM 案例](#bkmk_ipam)  
  
-   [NIC 小組案例](#bkmk_nicteam)

- [切換內嵌小組 \(SET @ no__t-2 案例](#bkmk_set)
  
### <a name="bkmk_sdn"></a>軟體定義網路（SDN）案例
 
您可以使用下列檔來部署具有 Windows Server 2016 的 SDN 案例。  
  
  
-   [使用腳本部署軟體定義的網路基礎結構](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
如需詳細資訊，請參閱[軟體&#40;定義&#41;網路 SDN](sdn/software-defined-networking.md)。  
  
#### <a name="bkmk_netc"></a>網路控制站案例

網路控制站案例可讓您：  
  
-   部署和管理網路控制站的多重節點實例。 如需詳細資訊，請參閱[使用 Windows PowerShell 部署網路控制](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)站。  
  
-   使用 [網路控制站]，透過 REST Northbound API 以程式設計方式定義網路原則。  
  
-   使用網路控制卡，透過 Hyper-v 網路虛擬化（使用 NVGRE 或 VXLAN 封裝）來建立和管理虛擬網路。  
  
如需詳細資訊，請參閱[網路控制卡](sdn/technologies/network-controller/Network-Controller.md)。  
  
#### <a name="bkmk_netf"></a>網路功能虛擬化（NFV）案例  
NFV 案例可讓您：  
  
-   部署並使用軟體負載平衡器，以分散 northbound 和 southbound 流量。  
  
-   部署並使用軟體負載平衡器，為使用 Hyper-v 網路虛擬化所建立的虛擬網路散發 eastbound 和 westbound 流量。  
  
-   針對使用 Hyper-v 網路虛擬化所建立的虛擬網路，部署和使用 NAT 軟體負載平衡器。  
  
-   部署和使用第3層轉送閘道  
  
-   針對站對站 IPsec （IKEv2）通道部署和使用虛擬私人網路（VPN）閘道  
  
-   部署和使用一般路由封裝（GRE）閘道。  
  
-   使用邊界閘道協定（BGP），在網站間部署和設定動態路由和傳輸路由。  
  
-   針對第3層和站對站閘道，以及 BGP 路由設定 M + N 冗余。  
  
-   使用網路控制卡來指定虛擬網路和網路介面上的 Acl。  
  
如需詳細資訊，請參閱[網路功能虛擬化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_netp"></a>網路平臺案例

在本節的案例中，Windows Server 網路小組支援使用任何 Windows Server 2016 認證的驅動程式。 請洽詢您的網路介面卡 \(NIC @ no__t-1 製造商，以確保您有最新的驅動程式更新。
  
網路平臺案例可讓您：  
  
-   使用交集 NIC，結合 RDMA 和 Ethernet 流量使用單一網路介面卡。  
  
-   使用「封包直接傳輸」（在 Hyper-v 虛擬交換器中啟用）和單一網路介面卡來建立低延遲的資料路徑。  
  
-   設定以將 SMB 直接傳輸和 RDMA 流量分散到兩個網路介面卡。  
  
如需詳細資訊，請參閱[遠端直接&#40;記憶體&#41;訪問 RDMA 和交換器&#40;內嵌&#41;小組集](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。  
  
#### <a name="bkmk_switch"></a>Hyper-v 虛擬交換器案例

Hyper-v 虛擬交換器案例可讓您：  
  
-   建立具有遠端直接記憶體存取（RDMA） vNIC 的 Hyper-v 虛擬交換器  
  
-   建立具有交換器內嵌小組（SET）和 RDMA Vnic 的 Hyper-v 虛擬交換器  
  
-   在 Hyper-v 虛擬交換器中建立集合小組  
  
-   使用 Windows PowerShell 命令來管理集合小組  
  
如需詳細資訊，請參閱[遠端直接&#40;記憶體&#41;訪問 RDMA 和交換器&#40;內嵌&#41;小組集](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>DNS 伺服器案例

DNS 伺服器案例可讓您：  
  
-   使用 DNS 原則來指定以地理位置為基礎的流量管理  
  
-   使用 DNS 原則設定分割大腦 DNS  
  
-   使用 DNS 原則對 DNS 查詢套用篩選  
  
-   使用 DNS 原則設定應用程式負載平衡  
  
-   根據當日時間指定智慧型 DNS 回應  
  
-   設定 DNS 區域轉送原則  
  
-   在 Active Directory Domain Services （AD DS）整合區域上設定 DNS 伺服器原則  
  
-   設定回應速率限制  
  
-   指定以 DNS 為基礎的命名實體驗證（來）  
  
-   設定 DNS 中的未知記錄支援  
  
如需詳細資訊，請參閱[Windows server 2016 中 Dns 用戶端的新功能](dns/What-s-New-in-DNS-Client.md)和[windows SERVER 2016 中 Dns 伺服器的新功能](dns/What-s-New-in-DNS-Server.md)主題。  
  
### <a name="bkmk_ipam"></a>使用 DHCP 和 DNS 的 IPAM 案例

IPAM 案例可讓您：  
  
-   探索和管理跨多個同盟 Active Directory 樹系的 DNS 和 DHCP 伺服器和 IP 位址  
  
-   使用 IPAM 進行 DNS 屬性的集中式管理，包括區域和資源記錄。  
  
-   定義細微的角色型存取控制原則，並委派 IPAM 使用者或使用者群組，以管理您指定的 DNS 屬性集。  
  
-   使用 IPAM 的 Windows PowerShell 命令，將 DHCP 和 DNS 的存取控制設定自動化。  
  
    如需詳細資訊，請參閱[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  
### <a name="bkmk_nicteam"></a>NIC 小組案例

NIC 小組案例可讓您：  
  
-   在支援的設定中建立 NIC 小組  
  
-   刪除 NIC 小組  
  
-   將網路介面卡新增至支援的設定中的 NIC 小組  
  
-   移除 NIC 小組的網路介面卡  
  
> [!NOTE]  
> 在 Windows Server 2016 中，您可以使用 Hyper-v 中的 NIC 小組，不過在某些情況下，虛擬機器佇列（VMQ）可能不會在您建立 NIC 小組時，于基礎網路介面卡上自動啟用。 如果發生這種情況，您可以使用下列 Windows PowerShell 命令，以確保在 NIC 小組成員介面卡上啟用 VMQ： `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

如需詳細資訊，請參閱[NIC](technologies/nic-teaming/NIC-Teaming.md)小組。 

### <a name="bkmk_set"></a>切換內嵌小組 \(SET @ no__t-2 案例

SET 是替代的 NIC 小組解決方案，可供您在 Windows Server 2016 中包含 Hyper-v 和軟體定義網路（SDN）堆疊的環境中使用。 將一些 NIC 小組功能整合到 Hyper-v 虛擬交換器。 

如需詳細資訊，請參閱[遠端直接記憶體存取（RDMA）和交換器內嵌小組（SET）](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>不支援的網路案例  
Windows Server 2016 不支援下列網路功能案例。  
  
-   以 VLAN 為基礎的租使用者虛擬網路。  
  
-   Underlay 或重迭中不支援 IPv6。  
  


