---
title: Windows Server 支援的網路功能案例
description: 本主題提供有關新支援網路功能案例在 Windows Server 2016 和更新版本資訊
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85f73f1f7caf833d23d3d693c0d754f52c4aa27d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812229"
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server 支援的網路功能案例

>適用於：Windows Server\(半年通道\)，Windows Server 2016

本主題提供支援和不受支援的案例，您可以或無法執行此版本的 Windows Server 2016 的相關資訊。  
>[!IMPORTANT]
>所有的生產案例中，使用最新已簽署的硬體驅動程式，從您的原始設備製造商\(OEM\)或 獨立硬體廠商\(IHV\)。
  
## <a name="bkmk_supp"></a>支援的網路功能案例

本章節包含適用於 Windows Server 2016，支援網路功能案例的相關資訊，並包含下列案例類別。  
  
-   [軟體定義網路 (SDN) 案例](#bkmk_sdn)  
  
-   [網路平台案例](#bkmk_netp)  
  
-   [DNS 伺服器案例](#bkmk_dns)  
  
-   [IPAM 與 DHCP 和 DNS 的案例](#bkmk_ipam)  
  
-   [NIC 小組的案例](#bkmk_nicteam)

- [交換器內嵌小組\(設定\)案例](#bkmk_set)
  
### <a name="bkmk_sdn"></a>軟體定義網路 (SDN) 案例
 
您可以使用下列文件，以部署 Windows Server 2016 的 SDN 案例。  
  
  
-   [部署軟體定義網路基礎結構使用指令碼](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
如需詳細資訊，請參閱 <<c0> [ 軟體定義網路&#40;SDN&#41;](sdn/software-defined-networking.md)。</c0>  
  
#### <a name="bkmk_netc"></a>網路控制站案例

網路控制站案例可讓您：  
  
-   部署和管理網路控制卡的多個節點執行個體。 如需詳細資訊，請參閱 <<c0> [ 部署網路控制站使用 Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
-   若要使用 REST Northbound API 以程式設計方式定義網路原則中使用網路控制站。  
  
-   若要建立和管理 HYPER-V 網路虛擬化-使用 NVGRE 或 VXLAN 封裝的虛擬網路使用網路控制站。  
  
如需詳細資訊，請參閱[網路控制卡](sdn/technologies/network-controller/Network-Controller.md)。  
  
#### <a name="bkmk_netf"></a>網路函式虛擬化 (NFV) 案例  
NFV 案例可讓您：  
  
-   部署與使用軟體負載平衡器將 northbound 和 southbound 流量。  
  
-   部署與使用軟體負載平衡器將 eastbound 和 westbound 使用 HYPER-V 網路虛擬化建立的虛擬網路的流量。  
  
-   部署與使用 NAT 軟體負載平衡器使用 HYPER-V 網路虛擬化建立的虛擬網路。  
  
-   部署和使用層級 3 轉寄閘道  
  
-   部署和使用虛擬私人網路 (VPN) 閘道的站對站 IPsec (IKEv2) 通道  
  
-   部署與使用 Generic Routing Encapsulation (GRE) 閘道。  
  
-   部署和設定動態路由和使用邊界閘道通訊協定 (BGP) 的站台之間的傳輸路由。  
  
-   第 3 層和站對站閘道和 BGP 路由，請設定 M + N 備援。  
  
-   使用網路控制站上的虛擬網路和網路介面指定 Acl。  
  
如需詳細資訊，請參閱 <<c0> [ 網路功能虛擬化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_netp"></a>網路平台案例

在本節中的 Windows Server Networking 案例小組支援認證的驅動程式的任何 Windows Server 2016 的使用。 請洽詢您的網路介面卡\(NIC\)製造商，以確保您擁有最新的驅動程式更新。
  
網路平台案例可讓您：  
  
-   您可以使用交集的 NIC，結合使用單一網路介面卡的 RDMA 和乙太網路流量。  
  
-   使用 Packet Direct，啟用在 HYPER-V 虛擬交換器，以及單一網路介面卡，以建立低延遲資料路徑。  
  
-   設定組分散 SMB 直接傳輸與 RDMA 最多兩個網路介面卡之間的流量流程。  
  
如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取&#40;RDMA&#41;和交換器內嵌小組&#40;設定&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。</c0>  
  
#### <a name="bkmk_switch"></a>HYPER-V 虛擬交換器案例

HYPER-V 虛擬交換器案例可讓您：  
  
-   使用遠端直接記憶體存取 (RDMA) vNIC 建立 HYPER-V 虛擬交換器  
  
-   使用 Switch Embedded Teaming (SET) 和 RDMA Vnic 建立 HYPER-V 虛擬交換器  
  
-   建立 HYPER-V 虛擬交換器中的設定小組  
  
-   使用 Windows PowerShell 命令來管理將小組設定  
  
如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取&#40;RDMA&#41;和交換器內嵌小組&#40;設定&#41;</c0>](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>DNS 伺服器案例

DNS 伺服器案例，可讓您：  
  
-   指定地理位置型流量管理使用 DNS 原則  
  
-   設定使用 DNS 原則拆分式 DNS  
  
-   使用 DNS 原則的 DNS 查詢上套用篩選  
  
-   設定應用程式負載平衡使用 DNS 原則  
  
-   指定的時間為基礎的智慧型 DNS 回應  
  
-   設定 DNS 區域傳輸原則  
  
-   設定 DNS 伺服器上的原則 Active Directory 網域服務 (AD DS) 整合區域  
  
-   設定回應速率限制  
  
-   指定的具名實體 （圖表） 的 DNS 為基礎的驗證  
  
-   在 DNS 中設定支援未知的記錄  
  
如需詳細資訊，請參閱主題[What's New in Windows Server 2016 中 DNS 用戶端](dns/What-s-New-in-DNS-Client.md)並[What's New in Windows Server 2016 中的 DNS 伺服器](dns/What-s-New-in-DNS-Server.md)。  
  
### <a name="bkmk_ipam"></a>IPAM 與 DHCP 和 DNS 的案例

IPAM 案例可讓您：  
  
-   探索和管理 DNS 和 DHCP 伺服器和跨多個同盟的 Active Directory 樹系的 IP 位址  
  
-   使用 IPAM 進行集中管理的 DNS 內容，包括區域和資源記錄。  
  
-   定義更細微的角色型存取控制原則和委派管理的一組您指定的 DNS 屬性的 IPAM 使用者或使用者群組。  
  
-   您可以使用 IPAM 的 Windows PowerShell 命令來自動化 DHCP 和 DNS 的存取控制設定。  
  
    如需詳細資訊，請參閱 <<c0> [ 管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  
### <a name="bkmk_nicteam"></a>NIC 小組的案例

NIC 小組的案例可讓您：  
  
-   支援的設定中建立的 NIC 小組  
  
-   刪除 NIC 小組  
  
-   新增至 NIC 小組支援的設定中的網路介面卡  
  
-   網路介面卡移除 NIC 小組  
  
> [!NOTE]  
> 在 Windows Server 2016 中，您可以使用 NIC 小組的 HYPER-V，但在某些情況下的虛擬機器佇列 (VMQ) 可能不會自動啟用基礎的網路介面卡上建立的 NIC 小組時。 如果發生這種情況，您可以使用下列 Windows PowerShell 命令來確認 VMQ 啟用 NIC 小組成員介面卡上： `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

如需詳細資訊，請參閱 < [NIC 小組](technologies/nic-teaming/NIC-Teaming.md)。 

### <a name="bkmk_set"></a>交換器內嵌小組\(設定\)案例

集合是替代的 NIC 小組解決方案，您可以使用包含 Windows Server 2016 中的 HYPER-V 和軟體定義網路 (SDN) 堆疊的環境中。 設定會將部分 NIC 小組功能整合到 HYPER-V 虛擬交換器。 

如需詳細資訊，請參閱[遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>不支援的網路功能案例  
Windows Server 2016 中不支援下列網路功能案例。  
  
-   VLAN 架構的租用戶虛擬網路。  
  
-   底圖或覆疊中不支援 IPv6。  
  


