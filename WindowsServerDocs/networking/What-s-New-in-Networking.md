---
title: 網路的新功能
description: 本主題提供有關新功能與技術的概觀資訊，Windows Server 2016 中的網路功能
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43ce6290f6559be7cb078032b79519d1681506d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829189"
---
# <a name="whats-new-in-networking"></a>網路的新功能

>適用於：Windows Server 2016

以下是 Windows Server 2016 中新的或增強網路技術。  
  Upd 本主題包含下列各節。  
  
-   [新的網路功能與技術](#bkmk_features)  
  
-   [額外的網路技術的新功能](#bkmk_existing)  
  
## <a name="bkmk_features"></a>新的網路功能與技術

網路功能是軟體定義資料中心 (SDDC) 平台的一個基本組件和 Windows Server 2016 提供全新和改進的軟體定義網路 (SDN) 技術，幫助您為您的組織移到完全實現的 SDDC 解決方案。  
  
當您以軟體定義的資源管理網路時，您可以描述應用程式的基礎結構需求一次，並選擇 應用程式執行所在的內部部署或雲端中。  這種一致性表示您的應用程式現在已調整更簡單，而且您可以順暢地執行具有同等的信心，周圍的安全性、 效能、 服務品質，以及可用性的應用程式，隨時隨地。  
  
下列各節包含這些資訊的新網路功能與技術。  
  
### <a name="software-defined-networking-infrastructure"></a>軟體定義網路基礎結構

以下是新增或改進的 SDN 基礎結構技術。  
  
-   **網路控制站**。 新增在 Windows Server 2016 中，網路控制站提供集中式、 可程式化的自動化點來管理、 設定、 監視和疑難排解您的資料中心內的虛擬和實體網路基礎結構。 使用網路控制器時，您可以自動化網路基礎結構的設定，而不用執行網路裝置與服務的手動設定。 如需詳細資訊，請參閱 <<c0> [ 網路控制卡](sdn/technologies/network-controller/Network-Controller.md)並[部署軟體定義網路使用指令碼](https://technet.microsoft.com/library/mt427380.aspx)。  
  
-   **HYPER-V 虛擬交換器**。 HYPER-V 虛擬交換器在 HYPER-V 主機上執行，並可讓您建立分散式的切換和路由、 和原則強制執行圖層，是對齊和與 Microsoft Azure 相容。 如需詳細資訊，請參閱 [Hyper-V 虛擬交換器](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
-   **網路功能虛擬化 (NFV)**。 在現今的軟體定義的資料中心，要由硬體設備 （例如負載平衡器、 防火牆、 路由器、 交換器等等） 的網路功能會逐漸被部署為虛擬設備。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。 虛擬設備是快速的新興和建立全新的市場。 它們繼續產生感興趣並取得趨勢電子報，在這兩個虛擬化平台上部署和雲端服務。 下列 NFV 技術可在 Windows Server 2016。  
  
    -   **資料中心防火牆**。 此分散式的防火牆提供更細微的存取控制清單 (Acl)，讓您套用 VM 介面層級或子網路層級的防火牆原則。  
  
        如需詳細資訊，請參閱 <<c0> [ 資料中心防火牆概觀](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  
    -   **RAS 閘道**。 您可以使用 RAS 閘道的虛擬網路與實體網路，包括站對站 VPN 連線，從您的雲端資料中心到您的租用戶的遠端站台之間路由傳送流量。 具體來說，您可以在其中部署網際網路金鑰交換版本 2 (IKEv2) 站台對站虛擬私人網路 (Vpn)、 第 3 層 (L3) VPN 和 Generic Routing Encapsulation (GRE) 閘道。 此外，現在支援閘道集區和 M + N 備援的閘道;和動態路由閘道的所有案例 （IKEv2 VPN、 GRE VPN 和 L3 VPN） 網路之間的邊界閘道通訊協定 (BGP) 路由反映程式功能提供。  
  
        如需詳細資訊，請參閱 < [What's New in RAS 閘道](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)並[適用於 SDN 的 RAS 閘道](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。  
          
    - **軟體負載平衡器 (SLB) 和網路位址轉譯 (NAT)**。 北南向調整大小和東-西層級 4 的負載平衡器和 NAT 強化產能藉由支援伺服器直接回傳，與傳回的網路流量可以略過負載平衡多工器。  
       如需詳細資訊，請參閱 <<c0> [ 軟體負載平衡&#40;SLB&#41;適用於 SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。</c0>  
  
    如需詳細資訊，請參閱 <<c0> [ 網路功能虛擬化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
-   **標準化的通訊協定**。 網路控制器 northbound 以 JavaScript Object Notation (JSON) 的承載介面上使用 Representational State Transfer (REST)。 網路控制卡 southbound 介面使用 Open vSwitch 資料庫管理通訊協定 (OVSDB)。  
  
-   **彈性的封裝技術**。 這些技術在資料平面，操作和支援虛擬可擴充 LAN (VxLAN) 和網路虛擬化 Generic Routing Encapsulation (NVGRE)。 如需詳細資訊，請參閱 < [Windows Server 2016 中的 GRE 通道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
如需 SDN 的詳細資訊，請參閱[軟體定義網路&#40;SDN&#41;](sdn/software-defined-networking.md)。  
  
### <a name="cloud-scale-fundamentals"></a>雲端規模的基本概念
 
現在提供下列雲端規模基本概念。  
  
-   **聚合式網路介面卡 (NIC)**。 交集的 NIC 可讓您用於管理、 啟用遠端直接記憶體存取 RDMA 的儲存體中的單一網路介面卡和租用戶流量。 因為您需要較少的網路介面卡，以管理不同類型的每一部伺服器的流量，這會減少您的資料中心，每一部伺服器相關聯的資本支出。  
  
-   **Packet Direct**。  Packet Direct 提供相當高的網路流量輸送量和低延遲封包處理基礎結構。  
  
-   **Switch Embedded Teaming (SET)**。        集合是 NIC 小組解決方案整合在 HYPER-V 虛擬交換器。 集合允許最多八個實體 NIC teaming 成單一組小組，這可改善可用性，並提供容錯移轉。 在 Windows Server 2016 中，您可以建立僅限於使用伺服器訊息區塊 (SMB) 和 RDMA 設定小組。 此外，您可以使用組小組，將針對 HYPER-V 網路虛擬化的網路流量。 如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取&#40;RDMA&#41;和交換器內嵌小組&#40;設定&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。</c0>  
  
## <a name="bkmk_existing"></a>額外的網路技術的新功能

本節包含熟悉的網路技術的新功能的相關資訊。
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP 是一項網際網路工程任務推動小組 (IETF) 標準，旨在減輕 TCP/IP 網路 (如私人內部網路) 上設定主機的管理負擔及複雜度。 使用 DHCP 伺服器服務可以將 DHCP 用戶端設定 TCP/IP 的程序自動化。  
  
如需詳細資訊，請參閱 < [What's New in DHCP](technologies/dhcp/What-s-New-in-DHCP.md)。  
  
## <a name="bkmk_dns"></a>DNS  
DNS 是 TCP/IP 網路中用來命名電腦及網路服務的系統。 DNS 命名會透過易記名稱找到電腦和服務。 當使用者在應用程式中輸入 DNS 名稱時，DNS 服務便可以將此名稱解析為與此名稱關聯的其他資訊 (如 IP 位址)。  
  
以下是 DNS 用戶端和 DNS 伺服器的相關資訊。  
  
### <a name="bkmk_dnsc"></a>DNS 用戶端  
以下是新增或改進的 DNS 用戶端技術。  
  
-   **DNS 用戶端服務繫結**。 在 Windows 10 中，DNS 用戶端服務會提供電腦與一個以上的網路介面的增強的支援。  
  
如需詳細資訊，請參閱[What's New in Windows Server 2016 中 DNS 用戶端](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>DNS 伺服器  
以下是新增或改進的 DNS 伺服器技術。  
  
-   **DNS 原則**。  您可以設定 DNS 原則，以指定的 DNS 伺服器回應 DNS 查詢的方式。 DNS 回應可以根據用戶端 IP 位址 （位置），時間的日期和數個其他參數。 DNS 原則可讓位置感知的 DNS、 流量管理、 負載平衡、 拆分式 DNS，以及其他案例。  
  
-   **Nano Server 支援檔案基礎 DNS**，您可以部署在 Windows Server 2016 Nano Server 映像上的 DNS 伺服器。 如果您使用此部署選項是您可以使用以檔案基礎的 DNS。 在 Nano Server 映像上執行 DNS 伺服器，您可以縮減、 快速啟動和最小化修補執行您的 DNS 伺服器。  
  
    > [!NOTE]   
    > Active Directory 整合 DNS 不支援 Nano Server 上。  
  
-   **回應速率限制 (RRL)**。  您可以讓您的 DNS 伺服器上的回應速率限制。 如此一來，您可以避免起始阻絕服務攻擊，DNS 用戶端上使用您的 DNS 伺服器的惡意系統的可能性。  
  
-   **具名實體 （圖表） 的 DNS 為基礎的驗證**。   您可以使用 TLSA （傳輸層安全性驗證） 的記錄資訊提供給 DNS 用戶端指出哪些憑證授權單位 (CA)，它們應該會從您的網域名稱的憑證。 這可防止有人可能會在此設定損毀的 DNS 快取，以指向自己的網站，並提供他們從不同的 CA 所發出之憑證的攔截層攻擊。  
  
-   **未知的記錄支援**。   
     您可以將明確使用未知的記錄功能的 Windows DNS 伺服器不支援記錄。  
  
-   **IPv6 根提示**。   
     您可以使用根提示來執行使用 IPV6 根伺服器的網際網路名稱解析所支援的原生 IPV6。  
  
-   **改進的 Windows PowerShell 支援**。   
      新的 Windows PowerShell cmdlet 可供 DNS 伺服器。  
  
如需詳細資訊，請參閱[What's New in Windows Server 2016 中的 DNS 伺服器](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>GRE 通道  
RAS 閘道現在支援高可用性 Generic Routing Encapsulation (GRE) 通道，為站對站連線及 M + N 備援的閘道。 GRE 是輕量級的通道通訊協定，可透過網際網路通訊協定網路封裝各種不同的虛擬點對點連結內的網路層通訊協定。  
  
如需詳細資訊，請參閱 < [Windows Server 2016 中的 GRE 通道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="HNV"></a>HYPER-V 網路虛擬化  
Windows Server 2012 中引入，HYPER-V 網路虛擬化 (HNV) 可讓共用的實體網路基礎結構之上將客戶網路虛擬化。 幾乎不需要變更需要在實體網路網狀架構上，HNV 讓服務提供者來部署和跨三個雲端的任何位置移轉租用戶工作負載彈性： 服務提供者雲端、 私人雲端或 Microsoft Azure 公用雲端。  
  
如需詳細資訊，請參閱[What's New in Windows Server 2016 中的 HYPER-V 網路虛擬化](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM 提供組織網路的 IP 位址和 DNS 基礎結構的高度可自訂的管理與監控功能。 您可以使用 IPAM，來監視、 稽核，以及管理執行動態主機設定通訊協定 (DHCP) 和網域名稱系統 (DNS) 伺服器。  
  
-   **增強的 IP 位址管理**。  
     IPAM 功能會改進處理 IPv4/32 和 IPv6/128 子網路和 IP 位址區塊中尋找可用的 IP 位址子網路和範圍等案例。  
  
-   **增強的 DNS 服務管理**。  
     IPAM 支援這兩個已加入網域的 Active Directory 整合和支援檔案的 DNS 伺服器的 DNS 資源記錄、 條件式轉寄站和 DNS 區域管理。  
  
-   **整合式 DNS、 DHCP 和 IP 位址 (DDI) 管理**。  
     有多項新體驗並啟用整合式生命週期管理作業，例如視覺化所有的 DNS 資源記錄 IP 的相關位址，根據 DNS 資源記錄，以及 IP 位址週期管理的 IP 位址的自動化的清查DNS 和 DHCP 的作業。  
  
-   **多個 Active Directory 樹系支援**。  
     您可以使用 IPAM 來管理多個 Active Directory 樹系的 DNS 和 DHCP 伺服器，IPAM 安裝所在的樹系和每個遠端樹系之間的雙向信任關係時。  
  
-   **角色型存取控制的 Windows PowerShell 支援**。  
     您可以使用 Windows PowerShell IPAM 物件上設定存取領域。  
  
如需詳細資訊，請參閱 < [What's New in IPAM](technologies/ipam/What-s-New-in-IPAM.md)並[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  

