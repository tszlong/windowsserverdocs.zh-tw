---
title: 網路功能
description: 本主題提供 Windows Server 2016 中可用的「軟體定義網路」和「網路平台」技術概觀。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: cf0aa1d752167962c11171ad98ae26591a740221
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868993"
---
# <a name="networking"></a>網路功能

>適用於：Windows Server (半年度管道)、Windows Server 2016

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> 網路功能是軟體定義的資料\(中心 SDDC\)平臺的基礎部分，而 Windows Server 2016 則提供全新且改良的軟體\)定義網路\(SDN 技術，協助您移至適用于貴組織的完全實現 SDDC 解決方案。

當您將網路當做軟體定義的資源來管理時，您可以一次描述應用程式的基礎結構需求，然後選擇應用程式在內部部署或雲端中的執行位置。 

這種一致性表示您的應用程式現在能夠輕易地進行擴充，而您可以隨時隨地順暢地執行應用程式，並在安全性、效能、服務品質及可用性方面具有同等的信心。

>[!Note]
> 若要下載 Windows Server，請參閱 [Windows Server 評估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview)。

Windows Server 2016 新增了下列新的網路技術：

- 軟體定義網路：網路控制站提供集中式、可程式化的自動化點，可以管理、設定、監視和疑難排解資料中心內的虛擬和實體網路基礎結構。 網路控制站可讓您使用網路功能虛擬化，輕鬆地\(部署\)虛擬機器 vm 以\(用於\)軟體負載平衡 SLB，以優化租使用者和 RAS 的網路流量負載閘道，為租使用者提供網際網路、內部內部部署和雲端資源之間所需的連線選項。 您也可以使用網路控制站，來管理 VM 和 Hyper-V 主機上的資料中心防火牆。

- 網路平臺：使用現有網路平臺技術的新功能，您可以使用 dns 原則自訂 dns 伺服器對查詢的回應、使用交集的 NIC 來處理合併的遠端直接記憶體\(訪問\) RDMA 和乙太網路流量， \(使用交換器內嵌小組集\)來建立連線至 RDMA nic 的 hyper-v 虛擬交換器，並使用 IP 位址管理\(IPAM\)來管理 DNS 區域和伺服器，以及 DHCP 和 IP 位址。

如需詳細資訊，請參閱 [Windows Server 支援的網路功能案例](windows-server-supported-networking-scenarios.md)。

下列章節提供 SDN 技術和網路平台技術的相關資訊。

## <a name="software-defined-networking-technologies"></a>軟體定義網路技術

### <a name="software-defined-networking-40sdn41sdnsoftware-defined-networkingmd"></a>[軟體定義網路&#40;SDN&#41;](sdn/software-defined-networking.md)

您可以使用本主題來了解 Windows Server、System Center 和 Microsoft Azure 中提供的 SDN 技術。

>[!NOTE]
>針對執行 SDN 基礎結構伺服器的 hyper-v \(主機\)和虛擬機器 vm （例如網路控制卡和軟體負載平衡節點），您必須安裝 Windows Server 2016 Datacenter edition。 對於僅包含連線到 SDN\-控制網路之租使用者工作負載 vm 的 hyper-v 主機，您可以執行 Windows Server 2016 Standard edition。

### <a name="deploy-a-software-defined-network-infrastructure-using-scriptssdndeploydeploy-a-software-defined-network-infrastructure-using-scriptsmd"></a>[使用腳本部署軟體定義的網路基礎結構](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
本指南提供如何在測試實驗室環境中，使用虛擬網路和閘道來部署網路控制站的相關指示。

### <a name="network-controllersdntechnologiesnetwork-controllernetwork-controllermd"></a>[網路控制卡](sdn/technologies/network-controller/Network-Controller.md)

網路控制站提供集中式、可程式化的自動化點，可以管理、設定、監視和疑難排解資料中心內的虛擬和實體網路基礎結構。

### <a name="software-load-balancing-40slb41-for-sdnsdntechnologiesnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[SDN 的軟體&#40;負載&#41;平衡 SLB](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

在 Windows Server \(2016\)中部署軟體定義網路（SDN）的雲端服務提供者 csp 和企業，可以使用軟體\(負載\)平衡 SLB 來平均散發租使用者和租使用者虛擬網路資源之間的客戶網路流量。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。

### <a name="ras-gateway-for-sdnsdntechnologiesnetwork-function-virtualizationras-gateway-for-sdnmd"></a>[適用於 SDN 的 RAS 閘道](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

RAS 閘道是 Windows Server 2016 中以軟體為基礎、 \(多\)租使用者、邊界閘道協定支援 BGP 的路由器，是專為裝載\(的\)雲端服務提供者 csp 和企業所設計使用 Hyper-v 網路虛擬化的多個租使用者虛擬網路。 

### <a name="network-function-virtualizationsdntechnologiesnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[網路功能虛擬化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

在軟體定義的資料中心，硬體設備\(（例如負載平衡器、防火牆、路由器、交換器\)等）所執行的網路功能，會逐漸虛擬化成為虛擬應用裝置。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。

### <a name="datacenter-firewall-overviewsdntechnologiesnetwork-function-virtualizationdatacenter-firewall-overviewmd"></a>[資料中心防火牆概觀](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

資料中心防火牆是一個網路層、5-Tuple (通訊協定、來源和目的地連接埠號碼，以及來源和目的地 IP 位址)、可設定狀態、多租用戶的防火牆。

## <a name="bkmk_networking"></a>網路技術

下表提供 Windows Server 2016 中一些網路技術的連結。

### <a name="whats-new-in-networkingnetworkingwhat-s-new-in-networkingmd"></a>[網路的新功能](../networking/What-s-New-in-Networking.md)

您可以運用下列章節來探索 Windows Server 2016 中的新網路技術，以及現有技術的新功能。

### <a name="branchcachebranchcachebranchcachemd"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache 是一種廣域網路\(WAN\)頻寬優化技術。 為了在使用者存取遠端伺服器的內容時將 WAN 頻寬最佳化，BranchCache 會從總公司或託管的雲端內容伺服器擷取內容，並在分公司快取內容，讓分公司的用戶端電腦可從本機存取內容而非透過 WAN。

### <a name="core-network-guide-for-windows-server-2016core-network-guidecore-network-guide-windows-servermd"></a>[Windows Server 2016 核心網路指南](core-network-guide/core-network-guide-windows-server.md)

了解如何利用「核心網路指南」部署 Windows Server 網路，以及利用「核心網路附屬指南」將功能新增至您的網路部署。

### <a name="directaccessremoteremote-accessdirectaccessdirectaccessmd"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess 可允許遠端使用者連線至組織網路資源。 

DirectAccess 文件現在位於 Windows Server 2016 目錄的[遠端存取和伺服器管理](https://docs.microsoft.com/windows-server/remote/)區段中，[遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access)下方。 如需詳細資訊，請參閱 [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)。

### <a name="domain-name-system-40dns41dnsdns-topmd"></a>[網域名稱系統&#40;DNS&#41;](dns/dns-top.md)

網域名稱系統 \(DNS\) 是其中一個構成 TCP/IP 通訊協定的業界標準套件，而且 DNS 用戶端和 DNS 伺服器可以一起為電腦和使用者提供電腦名稱到 IP 位址對應名稱解析服務。

### <a name="dynamic-host-configuration-protocol-40dhcp41technologiesdhcpdhcp-topmd"></a>[動態主機設定通訊協定&#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

動態主機設定通訊協定 \(DHCP\) 是會自動提供網際網路通訊協定 \(IP\) 主機及其 IP 位址和其他相關設定資訊 (例如子網路遮罩與預設閘道) 的用戶端/伺服器通訊協定。

### <a name="hyper-v-network-virtualizationsdntechnologieshyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Hyper-v 網路虛擬化](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Hyper-V 網路虛擬化 \(HNV\) 可以在共用的實體網路基礎結構之上將客戶網路虛擬化。

### <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Hyper-V 虛擬交換器](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Hyper-V 虛擬交換器是一種軟體式 Layer-2 乙太網路交換器，安裝 Hyper-V 伺服器角色時會隨附在 Hyper-V 管理員中。 交換器包含以程式設計方式管理和可擴充的功能，將虛擬機器連線到虛擬網路和實體網路。 此外，Hyper-V 虛擬交換器提供安全性、隔離以及服務層級的原則強化。 

Hyper-V 虛擬交換器文件現在位於 Windows Server 2016 目錄的 **「虛擬化」** 區段。 如需詳細資訊，請參閱 [Hyper-V 虛擬交換器](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。

### <a name="ip-address-management-40ipam41technologiesipamipam-topmd"></a>[IP 位址管理&#40;IPAM&#41;](technologies/ipam/ipam-top.md)

IP 位址管理 \(IPAM\) 是一個整合的工具套件，能夠在您 IP 位址基礎結構的端對端規劃、部署、管理及監視上提供豐富的使用者體驗。 IPAM 會自動探索您網路上的 IP 位址基礎\(結構\)伺服器和網域名稱系統 DNS 伺服器，並可讓您從中央介面管理它們。

### <a name="network-load-balancingtechnologiesnetwork-load-balancingmd"></a>[網路負載平衡](technologies/Network-Load-Balancing.md)

網路負載平衡 \(NLB\) 會使用 TCP/IP 網路通訊協定，將流量分散到數台伺服器。 針對非 SDN 的部署，NLB 可藉由在負載增加時新增額外的伺服器，來確保無狀態應用程式 (例如，執行網際網路資訊服務 \(IIS\) 的網頁伺服器) 是可擴充的。

### <a name="high-performance-networkingtechnologieshpnhpn-topmd"></a>[高效能網路功能](technologies/hpn/hpn-top.md)

Windows Server 2016 中的網路卸載和最佳化技術包含「僅限軟體」(SO) 功能和技術、「硬體和軟體」(SH) 整合功能和技術，以及「僅限硬體」(HO) 功能和技術。

此外也提供下列卸載和最佳化技術文件。

- [聚合式網路介面卡（NIC）設定指南](technologies/conv-nic/cnic-top.md)
- [資料中心橋接（DCB）](technologies/dcb/dcb-top.md)
- [虛擬接收端調整 (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-servertechnologiesnpsnps-topmd"></a>[網路原則伺服器](technologies/nps/nps-top.md)

網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。

### <a name="network-shell-netshtechnologiesnetshnetshmd"></a>[網路殼層 (Netsh)](technologies/netsh/netsh.md)

您可以使用 Network Shell \(netsh\)網路功能公用程式來管理 windows Server 2016 和 windows 10 中的網路技術。

### <a name="network-subsystem-performance-tuningtechnologiesnetwork-subsystemnet-sub-performance-topmd"></a>[網路子系統效能調整](technologies/network-subsystem/net-sub-performance-top.md)

本主題提供的資訊是關於為您的伺服器工作負載選擇正確的網路介面卡、排序網路介面、網路相關的效能計數器，以及效能調整網路介面卡和相關的網路技術，例如接收端調整\(RSS\)、接收端\(聯合 RSC\)及其他專案。

### <a name="nic-teamingtechnologiesnic-teamingnic-teamingmd"></a>[NIC 小組](technologies/nic-teaming/NIC-Teaming.md)

NIC 小組可讓您將實體的乙太網路介面卡群組為一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。

### <a name="quality-of-service-qos-policytechnologiesqosqos-policy-topmd"></a>[服務品質（QoS）原則](technologies/qos/qos-policy-top.md)

您可以透過建立 QoS 設定檔並使用群組原則發佈其設定，來使用 QoS 原則做為整體 Active Directory 基礎結構的網路頻寬管理中心點。

### <a name="remote-accessremoteremote-accessremote-accessmd"></a>[遠端存取](../remote/remote-access/remote-access.md)

您可以使用遠端存取技術（例如 DirectAccess 和虛擬私人網路\(VPN\) ），為遠端工作者提供內部網路資源的連線能力。 此外，您可以針對區域網路\(LAN\)路由和 Web 應用程式 Proxy 使用「遠端存取」。 這可為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以允許任何裝置上的使用者從公司網路外部存取這些應用程式。

「遠端存取」文件現在位於 Windows Server 2016 目錄的[遠端存取和伺服器管理區段](https://docs.microsoft.com/windows-server/remote/)中。 如需詳細資訊，請參閱[遠端存取](../remote/remote-access/remote-access.md)。

如需 Web 應用程式 Proxy (這是遠端存取伺服器角色的角色服務) 的詳細資訊，請參閱 [Windows Server 2016 中的 Web 應用程式 Proxy](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server)。

### <a name="virtual-private-networking-vpnremoteremote-accessvpnvpn-topmd"></a>[虛擬私人網路 (VPN)](../remote/remote-access/vpn/vpn-top.md)

在 Windows Server 2016 中，**DirectAccess 和 VPN** 是**遠端存取**伺服器角色的角色服務。

當您將遠端存取安裝為 VPN 伺服器時，您可以使用虛擬專用\(網\) VPN，讓您的遠端員工能夠透過網際網路連線到您的組織網路，同時維護資訊具有加密連接的隱私權。

運用 Windows Server 2016 遠端存取 VPN (以及 Windows 10 用戶端電腦)，您現在可以部署 Always On VPN。 Always On VPN 能讓您管理永遠保持連線的遠端 VPN 用戶端，同時也方便遠端工作者，讓他們不再需要手動連線和中斷連線您組織網路的 VPN。

如需詳細資訊，請參閱 [Windows Server 2016 和 Windows 10 的遠端存取 Always On VPN 部署指南](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)。

>[!NOTE]
>VPN 文件現在位於 Windows Server 2016 目錄的[遠端存取和伺服器管理](https://docs.microsoft.com/windows-server/remote/)區段中，[遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access)下方。

如需 VPN 的詳細資訊，請參閱[虛擬私人網路 (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top)。

### <a name="windows-container-networkinghttpsdocsmicrosoftcomvirtualizationwindowscontainersmanage-containerscontainer-networking"></a>[Windows 容器網路功能](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Windows 容器網路功能可讓您使用業界標準工具和工作流程，來建立和管理用於連線 Windows 10 和 Windows Server 主機上容器端點的網路。 Windows 容器網路支援多拓撲，包括私人、flat-L2 和 routed-L3。

也支援使用 Docker、Kubernetes 或 windows PowerShell 透過與 windows 主機網路服務\(HNS\)進行通訊的外掛程式，在主機本機上建立的重迭。 您可以透過本機代理程式\-與每個節點的 HNS 進行通訊，藉此建立和管理多節點叢集網路。

### <a name="windows-internet-name-service-winstechnologieswinswins-topmd"></a>[Windows 網際網路名稱服務 (WINS)](technologies/wins/wins-top.md)

Windows 網際網路名稱服務 (WINS) 是舊版電腦名稱登錄與解析服務，可將電腦 NetBIOS 名稱對應至 IP 位址。 建議使用 DNS 而不要使用 WINS。

## <a name="additional-resources"></a>其他資源

您可以在下列位置取得 Windows Server 2016 之前的作業系統網路資源。

- Windows Server 2012 和 Windows Server 2012 R2 [網路功能概觀](https://technet.microsoft.com/library/hh831357.aspx)
- Windows Server 2008 和 Windows Server 2008 R2 [網路功能](https://technet.microsoft.com/library/cc753940)
- Windows Server 2003 [Windows server 2003/2003 R2 已淘汰的內容](https://www.microsoft.com/download/details.aspx?id=53314)
