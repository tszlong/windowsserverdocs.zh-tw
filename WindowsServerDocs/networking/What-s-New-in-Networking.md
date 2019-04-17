---
title: 網路中的新功能
description: 本主題提供網路，在 Windows Server 2016 中的新功能和技術的相關的概觀的資訊
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85b1a573e8ac724a2a9e22863f890db668423cad
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-networking"></a>網路中的新功能

>適用於：Windows Server 2016

以下是 Windows Server 2016 中的新的或美化網路技術。  
  
本主題包含下列各節。  
  
-   [新的網路功能和技術](#bkmk_features)  
  
-   [其他網路技術的新功能](#bkmk_existing)  
  
## <a name="bkmk_features"></a>新的網路功能和技術

網路屬於基礎軟體定義 Datacenter (SDDC) 平台與 Windows Server 2016 提供新的和已改進軟體定義網路 (SDN) 技術，以協助您前往您的組織完全實現 SDDC 方案。  
  
當您軟體定義資源以管理網路時，您可以一次，描述應用程式的基礎結構需求，然後選擇位置的應用程式-場所或執行在雲端中。  這種一致性表示您的應用程式現在會更輕鬆地縮放，您可以順暢地進行執行的應用程式，隨時隨地周圍安全性效能、 服務及可用性品質相等信賴的。  
  
下列章節包含資訊這些新的網路功能和技術。  
  
### <a name="software-defined-networking-infrastructure"></a>軟體定義的網路基礎結構

以下是新的或改進 SDN 基礎結構技術。  
  
-   **網路控制器**。 新的 Windows Server 2016 中 Network Controller 提供管理、 設定、 監視，以及疑難排解 virtual 和實體網路基礎結構，在您的資料中心自動化打造、 程式化的點。 您可以使用網路控制器，請將網路基礎結構，而不是執行手動設定網路的裝置和服務的設定。 如需詳細資訊，請查看[Network Controller](sdn/technologies/network-controller/Network-Controller.md)和[部署軟體定義的網路使用指令碼](https://technet.microsoft.com/library/mt427380.aspx)。  
  
-   **HYPER-V Virtual 開關切換至**。 HYPER-V Virtual 開關切換至 HYPER-V 主機上執行，並且可讓您建立分散切換與路由原則執法層級，且對齊和 Microsoft Azure 相容。 如需詳細資訊，請查看[HYPER-V Virtual 切換](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
-   **網路功能模擬 (NFV)**。 在今天的軟體定義的資料中心，硬體裝置 （例如負載平衡器、 防火牆、 路由器、 參數，等等） 來執行網路功能越來越正在為 virtual 設備部署。 這「網路功能模擬」是伺服器模擬和網路模擬自然進展。 快速新興，建立的全新市場 virtual 裝置。 他們繼續產生興趣取得待發這兩個模擬平台和雲端服務。 Windows Server 2016 提供下列 NFV 技術。  
  
    -   **Datacenter 防火牆**。 這個分散式的防火牆提供細微存取控制清單 (Acl)，讓您用於防火牆原則，在 VM 介面層級或子網路層級。  
  
        如需詳細資訊，請查看[Datacenter 防火牆概觀](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。  
  
    -   **RAS 閘道**。 您可以使用 RAS 閘道路由之間的流量 virtual 網路和實體網路，包括-網站 VPN 來自雲端資料中心您 tenants' 遠端網站。 具體而言，您可以部署網際網路金鑰交換版本 (IKEv2) 2-網站 virtual 私人網路 (Vpn)、 VPN 層級 3 (L3)，和閘道一般路由封裝 (GRE)。 此外，現在支援閘道集區與 M + N 冗餘的閘道;並邊境閘道通訊協定 (BGP) 路由反映功能提供動態路由之間網路所有閘道案例 （IKEv2 VPN、 GRE VPN、 和 L3 VPN）。  
  
        如需詳細資訊，請查看[RAS 閘道] 中的新功能](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[RAS 閘道 SDN 的](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。  
          
    - **軟體負載平衡器 (SLB) 和網路位址轉譯 (NAT)**。 東西與北南層級 4 負載平衡器和 NAT 支援直接伺服器傳回，與退貨網路流量可以略過負載平衡多工器美化處理能力。  
       如需詳細資訊，請查看[軟體負載平衡和 #40;SLB 與 #41;適用於 SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
    如需詳細資訊，請查看[網路功能模擬](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。  
  
-   **標準化通訊協定**。 Network Controller JavaScript 物件標記 (JSON) 裝載其 northbound 介面使用代表狀態傳輸 （其餘部分）。 Network Controller southbound 介面使用開放 vSwitch 資料庫管理通訊協定 (OVSDB)。  
  
-   **封裝彈性技術**。 這些技術運作資料平面，並支援 Virtual 最具擴充性的區域網路 (VxLAN)，以及網路模擬一般路由封裝 (NVGRE)。 如需詳細資訊，請查看[在 Windows Server 2016 的 GRE 通道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
如需 SDN 的詳細資訊，請查看[軟體定義網路與 #40;SDN 與 #41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>雲端縮放比例基本概念
 
已可使用下列雲端縮放比例基本概念。  
  
-   **匯集網路介面卡 (NIC)**。 聚合型的而可讓您的單一網路介面卡用於管理，遠端直接記憶體存取 RDMA 式存放裝置及承租人傳輸。 這樣可以降低相關聯的資料中心的每個伺服器大寫費用，因為您需要管理不同類型的資料傳輸每個伺服器較少的網路介面卡。  
  
-   **封包直接**。  封包直接提供了高網路流量輸送量和低延遲封包處理基礎結構。  
  
-   **切換 Embedded 小組 （設定）**。        設定為 HYPER-V Virtual 切換中整合的小組 NIC 方案。 設定可讓成單一設定團隊，這可以改善可用性和提供容錯移轉的最多按的實體 NIC 小組。 您可以在 Windows Server 2016 建立會限制使用 RDMA 伺服器訊息區 (SMB) 的設定團隊。 此外，您可以將網路流量的 HYPER-V 網路模擬使用設定團隊。 如需詳細資訊，請查看[遠端直接記憶體存取和 #40;RDMA 與 #41;切換 Embedded 小組與 #40; 以及設定與 #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>其他網路技術的新功能

本節中的新功能熟悉網路技術的相關資訊。
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP 是設計用來減少的系統管理負擔和複雜的 TCP 型網路，例如私人內部網路設定主機網際網路工程設計工作推動 (IETF) 標準。 使用 DHCP 伺服器服務，程序的 TCP/IP 設定戶端 DHCP 會自動執行。  
  
如需詳細資訊，請查看[最新 dhcp](technologies/dhcp/What-s-New-in-DHCP.md)。  
  
## <a name="bkmk_dns"></a>DNS  
DNS 是使用適用於電腦與網路的服務命名 TCP/IP 網路中的系統。 DNS 命名找出電腦與服務，透過易記名稱。 當使用者應用程式中，輸入 DNS 名稱時，DNS 服務可以該名稱解析為名稱，例如 IP 位址相關的其他資訊。  
  
以下是 DNS Client 和 DNS 伺服器的資訊。  
  
### <a name="bkmk_dnsc"></a>DNS Client  
以下是新的或改進 DNS client 技術。  
  
-   **DNS Client 服務繫結**。 在 Windows 10 中，DNS Client 服務提供的電腦有多個網路介面美化的支援。  
  
如需詳細資訊，請查看[在 Windows Server 2016 DNS Client 中的新功能](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>DNS 伺服器  
以下是新的或改進的 DNS 伺服器技術。  
  
-   **DNS 原則**。  您可以設定 DNS 原則，若要指定 DNS 伺服器回應 DNS 查詢的方式。 DNS 回應可以根據 client IP 位址 （位置），以及幾個其他的參數時間。 定位感知 DNS、 流量管理、 負載平衡、 split-brain DNS 及其他案例，可讓 DNS 原則。  
  
-   **Nano Server 支援的檔案以 DNS**，您可以將 DNS 伺服器 Windows Server 2016 中的 Nano Server 映像上部署。 如果您使用此部署選項是可供您的檔案以 DNS。 Nano Server 映像上執行 DNS 伺服器，您可以減少的使用量、 與快速開機、 最小化修補執行您的 DNS 伺服器。  
  
    > [!NOTE]   
    > Active Directory 整合 DNS Nano Server 不支援。  
  
-   **回應評等限制 (RRL)**。  您可以讓您的 DNS 伺服器上的回應速率限制。 執行此動作，您避免使用您的 DNS 伺服器起始阻斷服務 DNS client 攻擊惡意系統。  
  
-   **DNS 驗證的命名實體 (DANE)**。   您可以使用 TLSA （傳輸層級的安全性驗證） 記錄 DNS 用狀態哪些憑證授權單位它們應該預期會從您的網域名稱的憑證，以提供的資訊。 如此可防止位置某人可能會損壞 DNS 快取指向贏得他們的網站，並提供他們所發行的其他 CA 憑證在中央男人攻擊。  
  
-   **無法辨識的記錄支援**。   
     您可以新增記錄明確不支援的 Windows DNS 伺服器使用未知的記錄功能。  
  
-   **IPv6 根提示**。   
     您可以使用的原生 IPV6 根提示支援執行網際網路名稱解析使用 IPV6 根伺服器。  
  
-   **已改善 Windows PowerShell 支援**。   
      新的 Windows PowerShell cmdlet 可用的 DNS 伺服器。  
  
如需詳細資訊，請查看[在 Windows Server 2016 中的 DNS 伺服器的新功能](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>GRE 通道  
RAS 閘道現在支援網站連接和閘道的 M + N 冗餘的可用性一般路由封裝 (GRE) 的通道。 GRE 是可透過網際網路通訊協定網路封裝各種不同的網路通訊協定 virtual 點對點連結中的輕量型通道通訊協定。  
  
如需詳細資訊，請查看[在 Windows Server 2016 的 GRE 通道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。  
  
## <a name="HNV"></a>HYPER-V 網路模擬  
HYPER-V 網路模擬 (HNV) 引進了 Windows Server 2012 中，可模擬客戶網路共用實體網路基礎結構上方。 實體網路 fabric 需要變更降到最低，HNV 提供服務提供者部署及三 cloud 上任何位置點一下移轉承租人工作負載靈敏度：雲端服務提供者、私人雲端，或是公用 Microsoft Azure 雲端。  
  
如需詳細資訊，請查看[在 Windows Server 2016 HYPER-V 網路模擬中的新功能](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM 提供組織網路的 IP 位址和 DNS 基礎結構高度自訂管理及監視功能。 使用 IPAM，您可以監視、 稽核，以及管理執行動態主機設定通訊協定 」 (DHCP) 和網域名稱系統 」 (DNS) 伺服器。  
  
-   **增強 IP 位址管理]**。  
     IPAM 功能的改良處理 IPv4/32 和 IPv6 /128 子網路和 IP 位址封鎖中尋找免費 IP 位址子網路和範圍案例。  
  
-   **增強 DNS 服務管理**。  
     IPAM 支援 DNS 資源記錄、 條件轉寄，以及 DNS 區域管理這兩個加入網域的 Active Directory 整合和檔案備份 DNS 伺服器。  
  
-   **整合 DNS、 DHCP 及 IP 位址 (DDI) 管理**。  
     幾個新體驗與整合式開發週期管理支援作業，例如視覺化所有 DNS 資源記錄屬於 IP 位址，自動清單的基礎 DNS 資源記錄及 IP 位址週期管理 DNS] 和 [DHCP 作業的 IP 位址。  
  
-   **多個 Active Directory 樹系支援**。  
     您可以使用 IPAM 雙向信任關係樹系安裝 IPAM，與每個遠端森林之間時管理多個 Active Directory 樹系的 DNS 及 DHCP 伺服器。  
  
-   **Windows PowerShell 角色根據存取控制支援**。  
     您可以使用 Windows PowerShell 來設定 IPAM 物件存取範圍。  
  
如需詳細資訊，請查看[新 IPAM 在](technologies/ipam/What-s-New-in-IPAM.md)和[管理 IPAM](technologies/ipam/Manage-IPAM.md)。  
  

