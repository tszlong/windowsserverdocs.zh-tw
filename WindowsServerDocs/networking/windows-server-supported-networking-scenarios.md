---
title: Windows Server 支援網路案例
description: 本主題提供新支援網路案例在 Windows Server 2016 及更新版本的相關資訊
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 70198f97c4ec39de4b78de28ab196dc3e86a684c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server 支援網路案例

>適用於：Windows Server \(Semi-Annual Channel\)、Windows Server 2016

本主題提供支援和不支援案例，您可以或無法執行此版本的 Windows Server 2016 的相關資訊。  
>[!IMPORTANT]
>適用於所有 production 案例中，使用您的原始設備製造商簽署最新的硬體驅動程式 \(OEM\) 或獨立硬體廠商 \(IHV\)。
  
## <a name="bkmk_supp"></a>網路案例的支援

本章節包含 Windows Server 2016 的網路支援案例相關資訊，並包含下列類案例。  
  
-   [軟體定義網路 (SDN) 案例](#bkmk_sdn)  
  
-   [網路平台案例](#bkmk_netp)  
  
-   [DNS 伺服器案例](#bkmk_dns)  
  
-   [使用 DHCP 和 DNS IPAM 案例](#bkmk_ipam)  
  
-   [NIC 小組案例](#bkmk_nicteam)

- [切換 Embedded 小組 \(SET\) 案例](#bkmk_set)
  
### <a name="bkmk_sdn"></a>軟體定義網路 (SDN) 案例
 
您可以使用下列的文件部署 SDN 與 Windows Server 2016 的案例。  
  
  
-   [部署軟體定義網路基礎結構使用指令碼，](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
如需詳細資訊，請查看[軟體定義網路與 #40;SDN 與 #41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>網路控制器案例

Network Controller 案例可讓您：  
  
-   部署及管理 Network Controller 的多節點執行個體。 如需詳細資訊，請查看[使用 Windows PowerShell 部署 Network Controller](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
-   使用 REST API Northbound 以程式設計方式定義的網路原則的網路使用控制器。  
  
-   使用網路控制器建立及管理 HYPER-V 網路模擬-使用 NVGRE 或 VXLAN 封裝 virtual 網路。  
  
如需詳細資訊，請查看[Network Controller](sdn/technologies/network-controller/Network-Controller.md)。  
  
#### <a name="bkmk_netf"></a>網路功能模擬 (NFV) 案例  
NFV 案例可讓您：  
  
-   部署，並使用軟體負載平衡器 northbound 和 southbound 流量。  
  
-   部署，並使用軟體負載平衡器 eastbound 和 westbound 建立 HYPER-V 網路模擬 virtual 網路流量。  
  
-   部署，並使用 NAT 軟體負載平衡器建立 HYPER-V 網路模擬 virtual 網路。  
  
-   部署，並使用層級 3 轉接閘道  
  
-   部署，並使用的私人網路 virtual (VPN) 閘道到網站 (IKEv2) IPsec 通道  
  
-   部署，並使用閘道一般路由封裝 (GRE)。  
  
-   部署，並設定動態路由和之間網站使用邊境閘道通訊協定 (BGP) 的移動路徑。  
  
-   設定 M + N 冗餘，層級 3 及-網站閘道和 BGP 路由。  
  
-   用於指定 Acl virtual 網路與網路介面網路控制器。  
  
如需詳細資訊，請查看[網路功能模擬](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_netp"></a>網路平台案例

Windows Server 網路本節中案例小組支援使用任何 Windows Server 2016 認證驅動程式。 請洽詢您的網路介面卡 \(NIC\) 製造商以確保您擁有最新的驅動程式更新。
  
網路平台案例可讓您：  
  
-   使用聚合型的 NIC 結合 RDMA 和乙太網路流量使用單一網路介面卡。  
  
-   建立低延遲資料路徑使用封包 Direct HYPER-V Virtual 開關和單一網路介面卡的支援。  
  
-   設定散播最多個網路介面卡之間 SMB 直接與 RDMA 流量的設定。  
  
如需詳細資訊，請查看[遠端直接記憶體存取和 #40;RDMA 與 #41;切換 Embedded 小組與 #40; 以及設定與 #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>HYPER-V Virtual 切換案例

HYPER-V Virtual 切換案例可讓您：  
  
-   使用遠端直接記憶體存取 (RDMA) 但 vNIC 建立 HYPER-V Virtual 開關切換至  
  
-   建立 HYPER-V Virtual 切換開關切換至 Embedded 小組（設定）和 RDMA vNICs 使用  
  
-   建立 HYPER-V Virtual 切換設定團隊  
  
-   使用 Windows PowerShell 命令來管理設定團隊  
  
如需詳細資訊，請查看[遠端直接記憶體存取和 #40;RDMA 與 #41;切換 Embedded 小組與 #40; 以及設定與 #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>DNS 伺服器案例

DNS 伺服器案例可讓您：  
  
-   指定地理位置型流量管理，使用 DNS 原則  
  
-   設定 split-brain DNS 使用 DNS 原則  
  
-   使用 DNS 原則 DNS 查詢上套用篩選  
  
-   設定應用程式負載平衡使用 DNS 原則  
  
-   指定智慧 DNS 回應根據一天的時間  
  
-   設定 DNS 區域傳輸原則  
  
-   設定 DNS 伺服器原則 Active Directory Domain Services (AD DS) 整合區域  
  
-   設定的回應速度限制  
  
-   指定 DNS 驗證的命名實體 (DANE)  
  
-   在 DNS 設定記錄未知的支援  
  
如需詳細資訊，請查看主題[在 Windows Server 2016 DNS Client 中的新功能](dns/What-s-New-in-DNS-Client.md)和[的 DNS 伺服器，在 Windows Server 2016 中的 [新增](dns/What-s-New-in-DNS-Server.md)。  
  
### <a name="bkmk_ipam"></a>使用 DHCP 和 DNS IPAM 案例

IPAM 案例可讓您：  
  
-   探索及管理 DNS 與 DHCP 伺服器以及跨多個聯盟 Active Directory 樹系的 IP 位址  
  
-   使用 IPAM DNS 屬性，包括區域和資源記錄的集中管理。  
  
-   定義細微以角色為基礎存取控制原則和委派 IPAM 使用者或群組使用者管理 DNS 屬性，指定的設定。  
  
-   使用 Windows PowerShell 命令 IPAM 自動化 DHCP 和 DNS 存取控制設定。  
  
    如需詳細資訊，請查看[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  
### <a name="bkmk_nicteam"></a>NIC 小組案例

NIC 小組案例可讓您：  
  
-   支援的設定建立 NIC 團隊  
  
-   Delete NIC 團隊  
  
-   網路介面卡加入 NIC 小組中支援的設定  
  
-   網路介面卡移除 NIC 小組  
  
> [!NOTE]  
> 在 Windows Server 2016 您可以使用 NIC 小組中 HYPER-V，但有時一樣佇列 (VMQ) 可能不會自動讓基礎網路介面卡上當您建立 NIC 團隊。 發生這種情形，如果您可以使用下列的 Windows PowerShell 命令，以確保 VMQ 尚未 NIC 小組成員介面卡上： `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

如需詳細資訊，請查看[NIC 小組](technologies/nic-teaming/NIC-Teaming.md)。 

### <a name="bkmk_set"></a>切換 Embedded 小組 \(SET\) 案例

設定為替代 NIC 小組方案，您可以在 Windows Server 2016 中包含 HYPER-V 和軟體所定義網路 (SDN) 堆疊的環境中使用。 設定 HYPER-V Virtual 開關切換至整合 NIC 小組的某些功能。 

如需詳細資訊，請查看[遠端直接記憶體存取 (RDMA) 和切換 Embedded 小組（設定）](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>不支援的網路案例  
Windows Server 2016 不支援下列網路功能案例。  
  
-   VLAN 型承租人 virtual 網路。  
  
-   底圖或覆疊 IPv6 不受支援。  
  


