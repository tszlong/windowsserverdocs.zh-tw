---
title: 網路的新功能
description: 本主題提供有關 Windows Server 2016 中網路功能的新功能和技術的總覽資訊。
ms.topic: how-to
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
author: dcuomo
ms.author: dacuo
ms.date: 08/07/2020
ms.openlocfilehash: 45654f1c75705bf4b1cc8b773527c37f11fb7f72
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949224"
---
# <a name="whats-new-in-networking"></a>網路的新功能

>適用於：Windows Server 2016

以下是 Windows Server 2016 中新增或增強的網路技術。
Upd 本主題包含下列各節。

-   [新的網路功能與技術](#bkmk_features)

-   [適用于其他網路技術的新功能](#bkmk_existing)

## <a name="new-networking-features-and-technologies"></a><a name="bkmk_features"></a>新的網路功能與技術

網路是軟體定義資料中心 (SDDC) 平臺的基礎元件，而 Windows Server 2016 則提供全新且改良的軟體定義網路 (SDN) 技術，以協助您移至組織的完全實現的 SDDC 解決方案。

當您以軟體定義的資源來管理網路時，您可以一次描述應用程式的基礎結構需求，然後選擇應用程式在內部部署或雲端中的執行位置。  這種一致性表示您的應用程式現在更容易調整，而且您可以在任何位置順暢地執行應用程式，並對安全性、效能、服務品質和可用性有同等的信賴度。

下列各節包含這些新網路功能和技術的相關資訊。

### <a name="software-defined-networking-infrastructure"></a>軟體定義網路基礎結構

以下是全新或改良的 SDN 基礎結構技術。

-   **網路控制** 站。 網路控制站是 Windows Server 2016 中的新功能，提供集中、可程式化的自動化點，以管理、設定、監視及疑難排解資料中心內的虛擬和實體網路基礎結構。 使用網路控制器時，您可以自動化網路基礎結構的設定，而不用執行網路裝置與服務的手動設定。 如需詳細資訊，請參閱 [網路控制](sdn/technologies/network-controller/Network-Controller.md) 站和 [使用腳本部署軟體定義的網路](/previous-versions/windows/server/mt427380(v=ws.12))。

-   **Hyper-v 虛擬交換器**。 Hyper-v 虛擬交換器是在 Hyper-v 主機上執行，可讓您建立分散式切換和路由，以及與 Microsoft Azure 一致且相容的原則強制層級。 如需詳細資訊，請參閱 [Hyper-V 虛擬交換器](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。

-   **Network Function Virtualization (NFV)**。 在現今的軟體定義資料中心中，硬體 (設備所執行的網路功能（例如負載平衡器、防火牆、路由器、) 交換器等等）會逐漸部署為虛擬裝置。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。 虛擬裝置會快速新興，並建立全新市場。 他們會持續產生興趣，並在虛擬化平臺和雲端服務中得到動力。 Windows Server 2016 提供下列 NFV 技術。

    -   **資料中心防火牆**。 此分散式防火牆會)  (Acl 提供細微的存取控制清單，可讓您在 VM 介面層級或子網層級套用防火牆原則。

        如需詳細資訊，請參閱 [資料中心防火牆總覽](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)。

    -   **RAS 閘道**。 您可以使用 RAS 閘道將虛擬網路與實體網路之間的流量路由傳送，包括從雲端資料中心到您租使用者遠端網站的站對站 VPN 連線。 具體來說，您可以部署網際網路金鑰交換第2版 (IKEv2) 站對站虛擬私人網路 (Vpn) 、第3層 (L3) VPN，以及泛型路由封裝 (GRE) 閘道。 此外，現在支援閘道集區和 M + N 個閘道的冗余;使用路由反映程式功能的邊界閘道協定 (BGP) 會提供網路之間的動態路由， (IKEv2 VPN、GRE VPN 和 L3 VPN) 的所有閘道案例。

        如需詳細資訊，請參閱  [Ras 閘道的新功能](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) 和 [SDN 的 ras 閘道](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)。

    - **軟體 Load Balancer (SLB) ，以及 (NAT) 的網路位址轉譯**。 北和東西部第4層負載平衡器和 NAT 藉由支援直接伺服器回傳來增強輸送量，讓傳回的網路流量可以略過負載平衡多工器。
       如需詳細資訊，請參閱 [SDN&#41; &#40;SLB 的軟體負載平衡](./sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)。

    如需詳細資訊，請參閱 [網路功能虛擬化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)。

-   **標準化的通訊協定**。 網路控制站會在其 northbound 介面上使用具象狀態傳輸 (REST) ，並 JavaScript 物件標記法 (JSON) 承載。 網路控制站 southbound 介面使用 Open vSwitch 資料庫管理通訊協定 (OVSDB) 。

-   **彈性的封裝技術**。 這些技術可在資料平面上運作，並且支援虛擬可擴充 LAN (VxLAN) 和網路虛擬化泛型路由封裝 (NVGRE) 。 如需詳細資訊，請參閱 [Windows Server 2016 中的 GRE 通道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。

如需 SDN 的詳細資訊，請參閱 [軟體定義的網路功能 &#40;sdn&#41;](sdn/software-defined-networking.md)。

### <a name="cloud-scale-fundamentals"></a>雲端規模基礎

現在提供下列雲端規模基礎概念。

-   **(NIC) 的融合式網路介面卡**。 融合式 NIC 可讓您使用單一網路介面卡進行管理、遠端直接記憶體存取 (RDMA) 啟用的儲存體和租使用者流量。 這可減少與您資料中心內每一部伺服器相關聯的資本支出，因為您需要較少的網路介面卡來管理每部伺服器的不同類型流量。

-   封 **包直接**。  封包直接提供高網路流量輸送量和低延遲的封包處理基礎結構。

-   **切換內嵌團隊 (設定)**。        SET 是 Hyper-v 虛擬交換器中整合的 NIC 小組解決方案。 設定可讓多達八個實體 NIC 組成單一集合的小組，以改善可用性並提供容錯移轉。 在 Windows Server 2016 中，您可以建立僅限於使用伺服器訊息區 (SMB) 和 RDMA 的設定團隊。 此外，您可以使用「設定小組」來散發 Hyper-v 網路虛擬化的網路流量。 如需詳細資訊，請參閱 [&#40;RDMA 的遠端直接記憶體存取&#41; 和切換內嵌小組 &#40;設定&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

## <a name="new-features-for-additional-networking-technologies"></a><a name="bkmk_existing"></a>適用于其他網路技術的新功能

本節包含熟悉的網路技術新功能的相關資訊。

## <a name="dhcp"></a><a name="bkmk_dhcp"></a>Dhcp
DHCP 是一項網際網路工程任務推動小組 (IETF) 標準，旨在減輕 TCP/IP 網路 (如私人內部網路) 上設定主機的管理負擔及複雜度。 使用 DHCP 伺服器服務可以將 DHCP 用戶端設定 TCP/IP 的程序自動化。

如需詳細資訊，請參閱 [DHCP 的新功能](technologies/dhcp/What-s-New-in-DHCP.md)。

## <a name="dns"></a><a name="bkmk_dns"></a>DNS
DNS 是 TCP/IP 網路中用來命名電腦及網路服務的系統。 DNS 命名會透過易記名稱找到電腦和服務。 當使用者在應用程式中輸入 DNS 名稱時，DNS 服務便可以將此名稱解析為與此名稱關聯的其他資訊 (如 IP 位址)。

以下是 DNS 用戶端和 DNS 伺服器的相關資訊。

### <a name="dns-client"></a><a name="bkmk_dnsc"></a>DNS 用戶端
以下是新增或改進的 DNS 用戶端技術。

-   **DNS 用戶端服務** 系結。 在 Windows 10 中，DNS 用戶端服務為具有多個網路介面的電腦提供增強的支援。

如需詳細資訊，請參閱 [Windows Server 2016 中 DNS 用戶端的新功能](dns/What-s-New-in-DNS-Client.md)

### <a name="dns-server"></a><a name="bkmk_dnss"></a>DNS 伺服器
以下是新增或改進的 DNS 伺服器技術。

-   **DNS 原則**。  您可以設定 DNS 原則，以指定 DNS 伺服器回應 DNS 查詢的方式。 DNS 回應可以根據用戶端 IP 位址 (位置) 、一天中的時間，以及數個其他參數。 DNS 原則會啟用位置感知 DNS、流量管理、負載平衡、分割腦 DNS 和其他案例。

-   **Nano server 支援以檔案為基礎的 dns**，您可以在 Windows server 2016 中的 nano server 映射上部署 DNS 伺服器。 如果您使用以檔案為基礎的 DNS，則可以使用此部署選項。 藉由在 Nano Server 映射上執行 DNS 伺服器，您可以使用較少的使用量、快速開機，以及最小化的修補來執行您的 DNS 伺服器。

    > [!NOTE]
    > Nano Server 不支援 Active Directory 整合式 DNS。

-   **回應速率限制 (的 RRL)**。  您可以在 DNS 伺服器上啟用回應速率限制。 如此一來，您就可以避免惡意系統使用您的 DNS 伺服器在 DNS 用戶端上起始阻絕服務攻擊的可能性。

-   **命名實體的 DNS 型驗證 (來)**。   您可以使用 TLSA (傳輸層安全性驗證) 記錄，將資訊提供給 DNS 用戶端，這些用戶端會向其指出您的功能變數名稱應預期憑證的證書 (頒發機構單位) 。 這可防止有人可能損毀 DNS 快取以指向自己網站的攔截式攻擊，並提供從不同 CA 發行的憑證。

-   **未知的記錄支援**。
     您可以使用未知的記錄功能來新增 Windows DNS 伺服器未明確支援的記錄。

-   **IPv6 根目錄提示**。
     您可以使用原生 IPV6 根提示支援，利用 IPV6 根功能變數名稱伺服器來執行網際網路名稱解析。

-   **改良的 Windows PowerShell 支援**。
      新的 Windows PowerShell Cmdlet 適用于 DNS 伺服器。

如需詳細資訊，請參閱 [Windows server 2016 中的 DNS 伺服器新功能](dns/What-s-New-in-DNS-Server.md)

## <a name="gre-tunneling"></a><a name="bkmk_GRE"></a>GRE 通道
RAS 閘道現在支援高可用性的一般路由封裝 (GRE) 通道進行站對站連線，以及 M + N 個閘道冗余。 GRE 是輕量級的通道通訊協定，可透過網際網路通訊協定網路封裝各種不同的虛擬點對點連結內的網路層通訊協定。

如需詳細資訊，請參閱 [Windows Server 2016 中的 GRE 通道](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md)。

## <a name="hyper-v-network-virtualization"></a><a name="HNV"></a>Hyper-v 網路虛擬化
Windows Server 2012 中引進的 Hyper-v 網路虛擬化 (HNV) 在共用實體網路基礎結構上啟用客戶網路虛擬化。 透過實體網路網狀架構所需的最少量變更，HNV 可讓服務提供者靈活地在三個雲端（服務提供者雲端、私用雲端或 Microsoft Azure 公用雲端）之間部署和遷移租使用者工作負載。

如需詳細資訊，請參閱 [Windows Server 2016 中 Hyper-v 網路虛擬化的新功能](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)

## <a name="ipam"></a><a name="bkmk_ipam"></a>IPAM
IPAM 針對組織網路上的 IP 位址和 DNS 基礎結構，提供高度可自訂的系統管理和監視功能。 您可以使用 IPAM 來監視、審核和管理執行動態主機設定通訊協定 (DHCP) 和網域名稱系統 (DNS) 的伺服器。

-   **增強的 IP 位址管理**。
     IPAM 功能已針對處理 IPv4/32 和 IPv6/128 子網，以及尋找 IP 位址區塊中的可用 IP 位址子網和範圍等案例進行改善。

-   **增強的 DNS 服務管理**。
     IPAM 支援 DNS 資源記錄、條件轉寄站，以及已加入網域 Active Directory 整合式和檔案支援的 DNS 伺服器的 DNS 區域管理。

-   **整合式 DNS、DHCP 和 IP 位址 (的 DDI) 管理**。
     已啟用數個新體驗和整合式生命週期管理作業，例如將所有與 IP 位址相關的 DNS 資源記錄視覺化、根據 DNS 資源記錄自動清查 IP 位址，以及 DNS 和 DHCP 作業的 IP 位址生命週期管理。

-   **多 Active Directory 樹系支援**。
     當 IPAM 安裝所在的樹系和每個遠端樹系之間有雙向信任關係時，您可以使用 IPAM 來管理多個 Active Directory 樹系的 DNS 和 DHCP 伺服器。

-   **Windows PowerShell 角色型存取控制的支援**。
     您可以使用 Windows PowerShell 來設定 IPAM 物件的存取範圍。

如需詳細資訊，請參閱 [ipam 的新功能](technologies/ipam/What-s-New-in-IPAM.md) 和 [管理 ipam](technologies/ipam/Manage-IPAM.md)。