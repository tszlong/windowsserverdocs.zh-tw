---
title: 軟體定義網路 (SDN)
description: 您可以使用本主題以深入了解在 Windows Server、 System Center 和 Microsoft Azure 會提供的軟體定義網路 (SDN) 技術。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33351ccfa1466f667ef9351768c89b373734075d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="software-defined-networking-sdn"></a>軟體定義網路 (SDN)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以深入了解在 Windows Server Datacenter edition、 System Center 2016 和 Microsoft Azure 會提供的軟體定義網路 (SDN) 技術。  
  
> [!NOTE]
> 本主題中，除了下列 SDN content 使用。
> 
> - [適用於 Windows Server SDN 中的新功能](sdn-whats-new.md)
> - [在 Windows Server 資料中心 SDN 簡介](sdn-intro.md)
> - [SDN 技術](technologies/Software-Defined-Networking-Technologies.md)  
> - [SDN 計劃](plan/Plan-Software-Defined-Networking.md) 
> - [部署 SDN](deploy/Deploy-Software-Defined-Networking.md)  
> - [管理 SDN](manage/manage-sdn.md)
> - [安全性 SDN](security/sdn-security-top.md)
> - [疑難排解 SDN](troubleshoot/Troubleshoot-Software-Defined-Networking.md)
> - [System Center SDN 技術](Sc-Tech-for-Sdn.md)
> - [Microsoft Azure 和 SDN](Azure_and_Sdn.md)
> - [請連絡雲端網路功能的小組與資料中心](contact-sdn-team.md)
  
## <a name="bkmk_sdn"></a>軟體定義網路概觀

軟體定義網路 \(SDN\) 提供的集中設定及管理 virtual 實體網路的裝置，例如路由器、 參數和閘道資料中心的方法。 HYPER-V Virtual 切換、 HYPER-V 網路模擬，和 RAS 閘道 virtual 網路元素的設計 SDN 基礎結構的不可或缺的項目。

>[!NOTE]
>HYPER-V 主機和虛擬機器 \(VMs\) 執行 SDN 基礎結構伺服器，例如 Network Controller and 軟體負載平衡節點，您必須安裝 Windows Server 2016 Datacenter edition。 HYPER-V 主機包含只承租人工作負載 Vm 連接 SDN\ 控制網路，您可以執行 Windows Server 2016 Standard edition。

當您仍然可以使用您現有的實體參數、 路由器，與其他硬體裝置時，您可以達到 virtual 網路之間的實體網路的深度整合這些裝置的設計與軟體定義網路的相容性。  
  
SDN 可能是因為網路飛機的管理、 控制和資料平面-不再繫結至網路的裝置，但的其他項目，例如 System Center 2016 datacenter 管理軟體的抽象使用。  
  
SDN 可讓您動態管理您的資料中心網路提供符合您的應用程式和工作負載的自動、 中央的方式。 網路定義軟體提供下列功能。  
  
-   抽象應用程式和基本的實體網路，透過虛擬化網路的工作負載的能力。 如同使用 HYPER-V server 模擬，抽象非常一致，您的應用程式和工作負載的非受到干擾的方式。 例如，軟體定義網路提供您實體網路項目，例如 IP 位址、 參數及負載平衡器 virtual 抽象。  
  
-   集中定義的功能和管理 virtual 和實體網路，包括這兩種網路類型間的流量控制項原則。  
  
-   實作一致的方式，縮放比例，網路原則，即使在您的部署新工作負載或將工作負載 virtual 或實體網路上的能力。  
  
## <a name="bkmk_ws"></a>Windows Server 技術軟體定義網路  
Windows Server 包含下列網路功能軟體定義技術。  

### <a name="bkmk_nc"></a>Network Controller

新的 Windows Server 2016 中 Network Controller 提供管理、 設定、 監視，以及疑難排解 virtual 兩自動化和實體網路基礎結構資料中心的集中、 程式化的點。 您可以使用網路控制器，請將網路基礎結構，而不是執行手動設定網路的裝置和服務的設定。  
  
Network Controller 的高度可用和擴充伺服器角色，並提供程式設計可進行通訊的 Network Controller 的網路介面 (API)-Southbound API-以及一個可讓您與 Network Controller 的第二個 API-Northbound API-一個應用程式。  
  
使用 Windows PowerShell、 代表狀態傳輸 （將） API 或管理應用程式，您可以使用 Network Controller 管理下列實體和 virtual 網路基礎結構。  
  
-   HYPER-V Vm 和 virtual 切換  
  
-   實體網路切換  
  
-   實體網路路由器  
  
-   防火牆軟體  
  
-   VPN 閘道，包括遠端存取服務 (RAS) Multitenant 閘道  
  
-   負載平衡器  
  
如需詳細資訊，請查看[Network Controller](../sdn/technologies/network-controller/Network-Controller.md)。  
  
### <a name="bkmk_hv"></a>HYPER-V 網路模擬

HYPER-V 網路模擬可協助您使用 virtual 網路抽象應用程式和實體網路的工作負載。 網路 virtual 提供共用實體網路 fabric，藉此駕駛資源使用量上執行時的必要 multitenant 隔離。 若要確保，您可以執行向前您現有的投資，您可以設定在現有的網路 gear virtual 網路。 此外，virtual 網路的區域網路 (Vlan) 相容。  
  
如需詳細資訊，請查看[HYPER-V 網路模擬](../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)。  
  
### <a name="bkmk_switch"></a>HYPER-V Virtual 開關切換至

HYPER-V Virtual 切換是軟體層級 2 乙太網路切換之後，您已安裝於 HYPER-V 伺服器角色 HYPER-V 管理員中可用的。 切換包含程式受管理和延伸虛擬電腦連接到 virtual 網路和實體網路功能。 此外，HYPER-V Virtual 切換提供原則執法的安全性、隔離與服務層級。  
  
在 Windows Server 2016 HYPER-V Virtual 切換，您也可以部署切換 Embedded 小組 （設定） 和遠端直接記憶體存取 (RDMA)。 如需詳細資訊，請查看區段[遠端直接記憶體存取 (RDMA) 和切換 Embedded 小組 （設定）](#bkmk_rdma)本主題中。  
  
如需 HYPER-V Virtual 切換的詳細資訊，請查看[HYPER-V Virtual 切換](../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。

### <a name="bkmk_idns"></a>內部 DNS 服務與 #40; Idn 和 #41;

裝載的虛擬機器 \(VMs\) 和應用程式需要 DNS 通訊在自己的網路和網際網路上的外部資源。 與 Idn，您可以使用 DNS 名稱解析服務提供 tenants 其名稱隔離的本機空間，以及網際網路資源。

如需詳細資訊，請查看[內部 DNS 服務與 #40; Idn 和 #41;適用於 SDN](technologies/Idns-for-Sdn.md)。
  
### <a name="bkmk_nfv"></a>網路功能模擬

在今天的軟體定義資料中心硬體裝置（例如負載平衡器、防火牆、路由器、參數，等等）來執行網路功能的越來越正在擬化檔案為 virtual 裝置。 這「網路功能模擬」是伺服器模擬和網路模擬自然進展。 快速新興，建立的全新市場 virtual 裝置。 他們繼續產生興趣取得待發這兩個模擬平台和雲端服務。  
  
可使用下列網路功能模擬技術。  
  
-   **軟體負載平衡器 (SLB) 和網路位址轉譯 (NAT)**。 東西與北南層級 4 負載平衡器和 NAT 支援直接伺服器傳回，與退貨網路流量可以略過負載平衡多工器美化處理能力。 如需詳細資訊，請查看[軟體負載平衡 (SLB) SDN 的](technologies/network-function-virtualization/software-load-balancing-for-sdn.md)。  
  
-   **Datacenter 防火牆**。 這個分散式的防火牆提供細微存取控制清單 (Acl)，讓您用於防火牆原則，在 VM 介面層級或子網路層級。  
  
    如需詳細資訊，請查看[Datacenter 防火牆概觀](../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  
-   **RAS 閘道**。 您可以使用閘道橋接網路 virtual 與非擬化檔案網路; 間的流量具體而言，您可以部署至網站 VPN 閘道、 轉接閘道和閘道一般路由封裝 (GRE)。 此外，M + N 冗餘閘道的支援。 如需詳細資訊，請查看[適用於 SDN RAS 閘道](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。
  
如需詳細資訊，請查看[網路功能模擬](technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
### <a name="bkmk_rdma"></a>遠端直接記憶體存取 (RDMA) 並切換 Embedded 小組 （設定）  
在 Windows Server 2016，您可以讓 RDMA 繫結至 HYPER-V Virtual 切換或不需要切換 Embedded 小組 （設定） 網路介面卡上。 這可讓您使用較少的網路介面卡，當您想要使用 RDMA 和一次。  
  
設定為替代 NIC 小組方案，您可以在 Windows Server 2016 中包含 HYPER-V 和軟體所定義網路 (SDN) 堆疊的環境中使用。 設定整合部分功能小組 NIC HYPER-V Virtual 開關切換至。  
  
設定可讓您一和八個實體乙太網路介面卡之間一或多個軟體 virtual 網路介面卡插入群組。 這些 virtual 網路介面卡提供快的效能與網路介面卡失敗容錯。  
設定成員網路介面卡必須所有安裝在相同的實體 HYPER-V 主機放在團隊。  
  
此外，您可以使用 Windows PowerShell 命令讓資料中心橋接 (DCB)、 建立 HYPER-V Virtual 切換與 RDMA virtual NIC (但 vNIC)，並建立 HYPER-V Virtual 切換的設定和 RDMA vNICs。  
  
如需詳細資訊，請查看[遠端直接記憶體存取 (RDMA) 和切換 Embedded 小組 （設定）](https://technet.microsoft.com/library/mt403349.aspx)。  
  
### <a name="bkmk_rras"></a>適用於 SDN RAS 閘道  
RAS 閘道為軟體，multitenant，邊境閘道通訊協定 (BGP) 可路由器專為雲端服務提供者 (Csp) 和主機多個承租人 virtual 網路使用 HYPER-V 網路模擬針對企業設計的 Windows Server 2016 中。  
  
RAS 閘道提供閘道集區，M + N 冗餘多種類型的網站來 VPN 連接和 BGP 路由反映為您提供具彈性的設計選擇閘道基礎結構。  
  
如需詳細資訊，請查看[適用於 SDN RAS 閘道](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
  
### <a name="bkmk_slb"></a>軟體負載平衡 (SLB)  
雲端服務提供者 (Csp) 與要部署的軟體定義網路 (SDN) 在 Windows Server 2016 中的企業可以使用軟體負載平衡 (SLB) 平均散發承租人和承租人客戶網路流量分配 virtual 網路資源。 Windows Server SLB 可讓伺服器多個主機相同的工作負載，可用性和延展性。  
  
如需詳細資訊，請查看[軟體負載平衡和 #40;SLB 與 #41;適用於 SDN](../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)。

### <a name="windows-server-containers"></a>Windows Server 容器

Windows Server 容器是用來與其他服務的容器主機上執行分開應用程式或服務的輕量型作業系統模擬方法。 若要於此，每個容器會有自己的作業系統，程序，檔案系統、登錄和 IP 位址的檢視。 與 Windows Server 2016，您現在可以連接 Windows Server 容器 virtual 網路。 如需詳細資訊，請查看[Windows Server 容器](technologies/containers/Container-networking-overview.md)。

### <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>請連絡 Datacenter 和雲端網路 product 小組

如果您感興趣討論 SDN 技術 Microsoft 或 SDN 技巧，有各種不同的方法製作連絡人。

如需詳細資訊，請查看[連絡雲端網路功能的小組與 Datacenter](contact-sdn-team.md)。
