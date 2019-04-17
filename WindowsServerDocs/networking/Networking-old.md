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
ms.openlocfilehash: 7caa99f1b6b9e25e5a6f2c4333b033fb3088195d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066838"
---
# 網路功能

>適用於：Windows Server (半年通道)、Windows Server 2016

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> 網路功能是軟體定義資料中心 \(SDDC\) 平台的一個基本組件，而 Windows Server 2016 提供新的和改進的軟體定義網路 \(SDN\) 技術，可協助您針對整個組織移至可完全實現的 SDDC 解決方案。

當您將網路當成軟體定義的資源來管理時，您可以說明應用程式的基礎結構需求一次，然後選擇應用程式執行的位置 (內部部署或在雲端中)。 

這種一致性表示您的應用程式現在能夠輕易地進行擴充，而您可以隨時隨地順暢地執行應用程式，並在安全性、效能、服務品質及可用性方面具有同等的信心。

>[!Note]
> 若要下載 Windows Server，請參閱 [Windows Server 評估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview)。

Windows Server 2016 新增了下列新的網路技術：

- 軟體定義網路：網路控制站提供集中式、可程式化的自動化點，可以管理、設定、監視和疑難排解資料中心內的虛擬和實體網路基礎結構。 網路控制站讓您能夠使用網路功能虛擬化，輕鬆地部署虛擬機器 \(VM\) 來取得軟體負載平衡 \(SLB\)，以針對您的租用戶來將網路流量負載最佳化，以及使用 RAS 閘道，為租用戶在網際網路、內部及雲端資源之間提供他們所需的連線選項。 您也可以使用網路控制站，來管理 VM 和 Hyper-V 主機上的資料中心防火牆。

- 網路平台︰使用現有網路平台技術的新功能，您可以使用 DNS 原則來自訂 DNS 伺服器對於查詢的回應、使用交集的 NIC 來處理合併的遠端直接記憶體存取 \(RDMA\) 和乙太網路流量、使用交換器內嵌小組 \(SET\) 來建立連線到 RDMA NIC 的 Hyper-V 虛擬交換器，以及使用 IP 位址管理 \(IPAM\) 來管理 DNS 區域和伺服器及 DHCP 和 IP 位址。

如需詳細資訊，請參閱 [Windows Server 支援的網路功能案例](windows-server-supported-networking-scenarios.md)。

下列章節提供 SDN 技術和網路平台技術的相關資訊。

## 軟體定義網路技術

### [軟體定義網路 &#40;SDN&#41;](sdn/software-defined-networking.md)

您可以使用本主題來了解 Windows Server、System Center 和 Microsoft Azure 中提供的 SDN 技術。

>[!NOTE]
>對於執行 SDN 基礎結構伺服器的 Hyper-V 主機和虛擬機器 \(VM\)，例如網路控制卡和軟體負載平衡節點，您必須安裝 Windows Server 2016 Datacenter Edition。 對於只包含連線到 SDN\ 控制網路之租用戶工作負載 VM 的 Hyper-V 主機，您可以執行 Windows Server 2016 Standard Edition。

### [使用指令碼部署軟體定義網路的基礎結構](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
本指南提供如何在測試實驗室環境中，使用虛擬網路和閘道來部署網路控制站的相關指示。

### [網路控制站](sdn/technologies/network-controller/Network-Controller.md)

網路控制站提供集中式、可程式化的自動化點，可以管理、設定、監視和疑難排解資料中心內的虛擬和實體網路基礎結構。

### [SDN 的軟體負載平衡 &#40;SLB&#41;](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

要在 Windows Server 2016 中部署軟體定義網路 (SDN) 的雲端服務提供者 \(CSP\) 和企業，可以使用軟體負載平衡 \(SLB\)，將租用戶和租用戶客戶網路流量平均地分散到虛擬網路資源。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。

### [適用於 SDN 的 RAS 閘道](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

RAS 閘道是 Windows Server 2016 中以軟體為基礎、多租用戶、具備邊界閘道通訊協定 \(BGP\) 功能的路由器，專為使用 Hyper-V 網路虛擬化裝載多個租用戶虛擬網路的雲端服務提供者 \(CSP\) 和企業而設計。 

### [網路函式虛擬化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

在軟體定義的資料中心，已將越來越多由硬體設備 \(例如負載平衡器、防火牆、路由器、交換器等\) 所執行的網路功能虛擬化為虛擬設備。 這個「網路功能虛擬化」是伺服器虛擬化和網路虛擬化的自然進展。

### [資料中心防火牆概觀](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

資料中心防火牆是一個網路層、5-Tuple (通訊協定、來源和目的地連接埠號碼，以及來源和目的地 IP 位址)、可設定狀態、多租用戶的防火牆。

## <a name="bkmk_networking"></a>網路技術

下表提供 Windows Server 2016 中一些網路技術的連結。

### [網路的新功能](../networking/What-s-New-in-Networking.md)

您可以運用下列章節來探索 Windows Server 2016 中的新網路技術，以及現有技術的新功能。

### [BranchCache](branchcache/BranchCache.md)

BranchCache 是廣域網路 \(WAN\) 頻寬最佳化技術。 為了在使用者存取遠端伺服器的內容時將 WAN 頻寬最佳化，BranchCache 會從總公司或託管的雲端內容伺服器擷取內容，並在分公司快取內容，讓分公司的用戶端電腦可從本機存取內容而非透過 WAN。

### [WindowsServer 2016 核心網路指南](core-network-guide/core-network-guide-windows-server.md)

了解如何利用「核心網路指南」部署 Windows Server 網路，以及利用「核心網路附屬指南」將功能新增至您的網路部署。

### [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess 可允許遠端使用者連線至組織網路資源。 

DirectAccess 文件現在位於 Windows Server 2016 目錄的[遠端存取和伺服器管理](https://docs.microsoft.com/windows-server/remote/)區段中，[遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access)下方。 如需詳細資訊，請參閱 [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)。

### [網域名稱系統 &#40;DNS&#41;](dns/dns-top.md)

網域名稱系統 \(DNS\) 是其中一個構成 TCP/IP 通訊協定的業界標準套件，而且 DNS 用戶端和 DNS 伺服器可以一起為電腦和使用者提供電腦名稱到 IP 位址對應名稱解析服務。

### [動態主機設定通訊協定 &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

動態主機設定通訊協定 \(DHCP\) 是會自動提供網際網路通訊協定 \(IP\) 主機及其 IP 位址和其他相關設定資訊 (例如子網路遮罩與預設閘道) 的用戶端/伺服器通訊協定。

### [Hyper-V 網路虛擬化](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Hyper-V 網路虛擬化 \(HNV\) 可以在共用的實體網路基礎結構之上將客戶網路虛擬化。

### [Hyper-V 虛擬交換器](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Hyper-V 虛擬交換器是一種軟體式 Layer-2 乙太網路交換器，安裝 Hyper-V 伺服器角色時會隨附在 Hyper-V 管理員中。 交換器包含以程式設計方式管理和可擴充的功能，將虛擬機器連線到虛擬網路和實體網路。 此外，Hyper-V 虛擬交換器提供安全性、隔離以及服務層級的原則強化。 

Hyper-V 虛擬交換器文件現在位於 Windows Server 2016 目錄的 **「虛擬化」** 區段。 如需詳細資訊，請參閱 [Hyper-V 虛擬交換器](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。

### [IP 位址管理 &#40;IPAM&#41;](technologies/ipam/ipam-top.md)

IP 位址管理 \(IPAM\) 是一個整合的工具套件，能夠在您 IP 位址基礎結構的端對端規劃、部署、管理及監視上提供豐富的使用者體驗。 IPAM 會自動探索您網路上的 IP 位址基礎結構伺服器和網域名稱系統 \(DNS\) 伺服器，並可讓您從中央介面管理這些伺服器。

### [網路負載平衡](technologies/Network-Load-Balancing.md)

網路負載平衡 \(NLB\) 會使用 TCP/IP 網路通訊協定，將流量分散到數台伺服器。 針對非 SDN 的部署，NLB 可藉由在負載增加時新增額外的伺服器，來確保無狀態應用程式 (例如，執行 Internet Information Services \(IIS\) 的網頁伺服器) 是可擴充的。

### [高效能網路功能](technologies/hpn/hpn-top.md)

Windows Server 2016 中的網路卸載和最佳化技術包含「僅限軟體」(SO) 功能和技術、「硬體和軟體」(SH) 整合功能和技術，以及「僅限硬體」(HO) 功能和技術。

此外也提供下列卸載和最佳化技術文件。

- [Converged Network Interface Card (NIC) 設定指南](technologies/conv-nic/cnic-top.md)
- [資料中心橋接 (DCB)](technologies/dcb/dcb-top.md)
- [虛擬接收端調整 (vRSS)](technologies/vrss/vrss-top.md)


### [網路原則伺服器](technologies/nps/nps-top.md)

網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。

### [網路殼層 (Netsh)](technologies/netsh/netsh.md)

您可以使用網路殼層 \(netsh\) 網路公用程式來管理 Windows Server 2016 和 Windows 10 中的網路技術。

### [網路子系統效能調整](technologies/network-subsystem/net-sub-performance-top.md)

本主題提供如何為您的伺服器工作負載選擇正確的網路介面卡、訂購網路介面卡、網路相關效能計數器、效能調整網路介面卡以及相關網路技術 (例如，接收端調整 \(RSS\)、接收端聯合 (RSC) 及其他項目) 的相關資訊。

### [NIC 小組](technologies/nic-teaming/NIC-Teaming.md)

NIC 小組可讓您將實體的乙太網路介面卡群組為一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。

### [服務品質 (QoS) 原則](technologies/qos/qos-policy-top.md)

您可以透過建立 QoS 設定檔並使用群組原則發佈其設定，來使用 QoS 原則做為整體 Active Directory 基礎結構的網路頻寬管理中心點。

### [遠端存取](../remote/remote-access/remote-access.md)

您可以使用遠端存取技術，例如 DirectAccess 和虛擬私人網路 \(VPN\) 來提供遠端網路使用者連線至內部網路資源。 此外，您還可以使用遠端存取做為區域網路 \(LAN\) 路由以及用於 Web 應用程式 Proxy。 這可為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以允許任何裝置上的使用者從公司網路外部存取這些應用程式。

「遠端存取」文件現在位於 Windows Server 2016 目錄的[遠端存取和伺服器管理區段](https://docs.microsoft.com/windows-server/remote/)中。 如需詳細資訊，請參閱[遠端存取](../remote/remote-access/remote-access.md)。

如需 Web 應用程式 Proxy (這是遠端存取伺服器角色的角色服務) 的詳細資訊，請參閱 [Windows Server 2016 中的 Web 應用程式 Proxy](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server)。

### [虛擬私人網路 (VPN)](../remote/remote-access/vpn/vpn-top.md)

在 Windows Server 2016 中，**DirectAccess 和 VPN** 是**遠端存取**伺服器角色的角色服務。

將遠端存取安裝為 VPN 伺服器時，您可以使用虛擬私人網路 \(VPN\) 讓遠端員工透過網際網路連線到您的組織網路，同時也能透過加密連線維護資訊隱私。

運用 Windows Server 2016 遠端存取 VPN (以及 Windows 10 用戶端電腦)，您現在可以部署 Always On VPN。 Always On VPN 能讓您管理永遠保持連線的遠端 VPN 用戶端，同時也方便遠端工作者，讓他們不再需要手動連線和中斷連線您組織網路的 VPN。

如需詳細資訊，請參閱 [Windows Server 2016 和 Windows 10 的遠端存取 Always On VPN 部署指南](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)。

>[!NOTE]
>VPN 文件現在位於 Windows Server 2016 目錄的[遠端存取和伺服器管理](https://docs.microsoft.com/windows-server/remote/)區段中，[遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access)下方。

如需 VPN 的詳細資訊，請參閱[虛擬私人網路 (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top)。

### [Windows 容器的網路功能](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

Windows 容器網路功能可讓您使用業界標準工具和工作流程，來建立和管理用於連線 Windows 10 和 Windows Server 主機上容器端點的網路。 Windows 容器網路支援多拓撲，包括私人、flat-L2 和 routed-L3。

也支援覆疊 (您可以使用 Docker、Kubernetes 或 Windows PowerShell，透過與 Windows 主機網路服務 \(HNS\) 通訊的外掛程式在本機主機上建立)。 您可以透過較高層級的協調系統，透過每個節點的 HNS 本機代理程式通訊，來建立及管理多節點叢集網路。

### [Windows 網際網路名稱服務 (WINS)](technologies/wins/wins-top.md)

Windows 網際網路名稱服務 (WINS) 是舊版電腦名稱登錄與解析服務，可將電腦 NetBIOS 名稱對應至 IP 位址。 建議使用 DNS 而不要使用 WINS。

## 其他資源

您可以在下列位置取得 Windows Server 2016 之前的作業系統網路資源。

- Windows Server 2012 和 Windows Server 2012 R2 [網路功能概觀](https://technet.microsoft.com/library/hh831357.aspx)
- Windows Server 2008 和 Windows Server 2008 R2 [網路功能](https://technet.microsoft.com/library/cc753940)
- Windows Server 2003 [Windows Server 2003/2003 R2 停用內容](https://www.microsoft.com/download/details.aspx?id=53314)
