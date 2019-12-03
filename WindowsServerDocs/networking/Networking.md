---
title: 網路
description: 本主題提供 Windows Server 2016 中可用的「軟體定義網路」和「網路平台」技術概觀。
ms.prod: windows-server
layout: LandingPage
ms.technology: networking
ms.topic: landing-page
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: ac6d9abf96348a596fbd2f886cb67b5d97f686e2
ms.sourcegitcommit: cbf0c7c37797c22af989639fac82fc0eee94497f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2019
ms.locfileid: "74700151"
---
# <a name="networking"></a>網路

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<hr />

網路功能是軟體定義資料中心 \(SDDC\) 平臺的基礎部分，而 Windows Server 2016 則提供全新和改良的軟體定義網路 \(SDN\) 技術，協助您為組織移至完全實現的 SDDC 解決方案。

當您將網路當做軟體定義的資源來管理時，您可以一次描述應用程式的基礎結構需求，然後選擇應用程式在內部部署或雲端中的執行位置。 

這種一致性表示您的應用程式現在能夠輕易地進行擴充，而您可以隨時隨地順暢地執行應用程式，並在安全性、效能、服務品質及可用性方面具有同等的信心。

<hr />

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-whats-new.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h2><a href="../networking/What-s-New-in-Networking.md">網路&#39;中的新功能</a></h2>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

<h2>軟體定義的網路功能</h2>

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/windows-server/networking/sdn/">軟體定義網路 (SDN)</a></h3>
                        <hr />
                        <p>您可以使用本主題來了解 Windows Server、System Center 和 Microsoft Azure 中提供的 SDN 技術。</p>
                        <p><b>注意：</b>對於執行 SDN 基礎結構伺服器的 Hyper-v 主機和虛擬機器（Vm）（例如網路控制卡和軟體負載平衡節點），您必須安裝 Windows Server Datacenter edition。 對於僅包含連線到 SDN 控制網路之租使用者工作負載 Vm 的 Hyper-v 主機，您可以執行 Windows Server Standard edition。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3><a href="sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md">使用腳本部署軟體定義的網路基礎結構</a></h3>
                    <hr />
                    <p>本指南提供如何在測試實驗室環境中，使用虛擬網路和閘道來部署網路控制站的相關指示。</p>
                </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-controller/Network-Controller.md">網路控制卡</a></h3>
                        <hr />
                        <p>網路控制站提供集中式、可程式化的自動化點，可以管理、設定、監視和疑難排解資料中心內的虛擬和實體網路基礎結構。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md">SDN 的軟體&#40;負載&#41;平衡 SLB</a></h3>
                        <hr />
                        <p>在 Windows Server 2016 中部署軟體定義網路（SDN）的雲端服務提供者（Csp）和企業可以使用軟體負載平衡（SLB），將租使用者和租使用者客戶網路流量平均分散到虛擬網路資源。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md">適用於 SDN 的 RAS 閘道</a></h3>
                        <hr />
                        <p>RAS 閘道是 Windows Server 2016 中以軟體為基礎、多租使用者、邊界閘道協定（BGP）功能的路由器，是針對雲端服務提供者（Csp）和使用 Hyper-v 網路裝載多個租使用者虛擬網路的企業所設計。領域.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md">網路功能虛擬化</a></h3>
                        <hr />
                        <p>在軟體定義的資料中心，硬體設備（例如負載平衡器、防火牆、路由器、交換器等）所執行的網路功能會逐漸虛擬化成為虛擬應用裝置。 這 &quot;網路功能虛擬化&quot; 是伺服器虛擬化和網路虛擬化的自然進展。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md">資料中心防火牆概觀</a></h3>
                        <hr />
                        <p>資料中心防火牆是一個網路層、5-Tuple (通訊協定、來源和目的地連接埠號碼，以及來源和目的地 IP 位址)、可設定狀態、多租用戶的防火牆。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

## <a name="bkmk_networking"></a>網路技術

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="branchcache/BranchCache.md">BranchCache</a></h3>
                        <hr />
                        <p>BranchCache 是廣域網路 (WAN) 頻寬最佳化技術。 為了在使用者存取遠端伺服器的內容時將 WAN 頻寬最佳化，BranchCache 會從總公司或託管的雲端內容伺服器擷取內容，並在分公司快取內容，讓分公司的用戶端電腦可從本機存取內容而非透過 WAN。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="core-network-guide/core-network-guide-windows-server.md">核心網路指南</a></h3>
                        <hr />
                        <p>了解如何利用「核心網路指南」部署 Windows Server 網路，以及利用「核心網路附屬指南」將功能新增至您的網路部署。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/directaccess/DirectAccess.md">DirectAccess</a></h3>
                        <hr />
                        <p>DirectAccess 可允許遠端使用者連線至組織網路資源。 </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="dns/dns-top.md">網域名稱系統（DNS）&quot;&gt;</a></h3>
                        <hr />
                        <p>網域名稱系統（DNS）是由 TCP/IP 組成的業界標準通訊協定套件之一，而且 DNS 用戶端和 DNS 伺服器會將電腦名稱稱到 IP 位址對應名稱解析服務提供給電腦和使用者。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/dhcp/dhcp-top.md">動態主機設定通訊協定&#40;DHCP&#41;</a></h3>
                        <hr />
                        <p>動態主機設定通訊協定（DHCP）是一種用戶端/伺服器通訊協定，它會自動提供網際網路通訊協定（IP）主機及其 IP 位址和其他相關的設定資訊，例如子網路遮罩和預設閘道。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md">Hyper-v 網路虛擬化</a></h3>
                        <hr />
                        <p>Hyper-v 網路虛擬化（HNV）可在共用的實體網路基礎結構之上，將客戶網路虛擬化。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">Hyper-V 虛擬交換器</a></h3>
                        <hr />
                        <p>Hyper-V 虛擬交換器是一種軟體式 Layer-2 乙太網路交換器，安裝 Hyper-V 伺服器角色時會隨附在 Hyper-V 管理員中。 交換器包含以程式設計方式管理和可擴充的功能，將虛擬機器連線到虛擬網路和實體網路。 此外，Hyper-V 虛擬交換器提供安全性、隔離以及服務層級的原則強化。 </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/ipam/ipam-top.md">IP 位址管理&#40;IPAM&#41;</a></h3>
                        <hr />
                        <p>IP 位址管理（IPAM）是一個整合的工具套件，可讓您透過豐富的使用者體驗，進行端對端規劃、部署、管理和監視您的 IP 位址基礎結構。 IPAM 會自動探索您網路上的 IP 位址基礎結構伺服器和網域名稱系統 (DNS) 伺服器，並可讓您從中央介面管理這些伺服器。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/Network-Load-Balancing.md">網路負載平衡</a></h3>
                        <hr />
                        <p>網路負載平衡（NLB）會使用 TCP/IP 網路通訊協定將流量分散到多部伺服器。 針對非 SDN 部署，NLB 可確保無狀態應用程式（例如執行 Internet Information Services （IIS）的 Web 服務器）可在負載增加時新增更多伺服器來調整。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/hpn/hpn-top.md">高效能網路功能</a></h3>
                        <hr />
                        <p>Windows Server 2016 中的網路卸載和最佳化技術包含「僅限軟體」(SO) 功能和技術、「硬體和軟體」(SH) 整合功能和技術，以及「僅限硬體」(HO) 功能和技術。</p>
                        <p>此外也提供下列卸載和最佳化技術文件：<p>
                        <hr />
                        <a href="technologies/conv-nic/cnic-top.md">高效能網路</a>
                        <hr />
                        <a href="technologies/dcb/dcb-top.md">資料中心橋接（DCB）</a>
                        <hr />
                        <a href="technologies/vrss/vrss-top.md">虛擬接收端調整（vRSS）</a>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nps/nps-top.md">網路原則伺服器</a></h3>
                        <hr />
                        <p>網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/netsh/netsh.md">網路殼層 (Netsh)</a></h3>
                        <hr />
                        <p>您可以使用 Network Shell （netsh）網路公用程式來管理 Windows Server 2016 和 Windows 10 中的網路技術。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/network-subsystem/net-sub-performance-top.md">網路子系統效能調整</a></h3>
                        <hr />
                        <p>本主題提供的資訊是關於為您的伺服器工作負載選擇正確的網路介面卡、排序網路介面、網路相關的效能計數器，以及效能調整網路介面卡和相關的網路技術，例如接收端調整（RSS）、接收端聯合（RSC），以及其他專案。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nic-teaming/NIC-Teaming.md">NIC 小組</a></h3>
                        <hr />
                        <p>NIC 小組可讓您將實體的乙太網路介面卡群組為一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/qos/qos-policy-top.md">服務品質（QoS）原則</a></h3>
                        <hr />
                        <p>您可以透過建立 QoS 設定檔並使用群組原則發佈其設定，來使用 QoS 原則做為整體 Active Directory 基礎結構的網路頻寬管理中心點。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/wins/wins-top.md">Windows 網際網路名稱服務 (WINS)</a></h3>
                        <hr />
                        <p>Windows 網際網路名稱服務 (WINS) 是舊版電腦名稱登錄與解析服務，可將電腦 NetBIOS 名稱對應至 IP 位址。 建議使用 DNS 而不要使用 WINS。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/remote-access.md">遠端存取</a></h3>
                        <hr />
                        <p>您可以使用遠端存取技術（例如 DirectAccess 和虛擬私人網路（VPN）），為遠端工作者提供內部網路資源的連線能力。 此外，您可以使用區域網路（LAN）路由和 Web 應用程式 Proxy 的遠端存取。 這可為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以允許任何裝置上的使用者從公司網路外部存取這些應用程式。</p>
                        <p>如需 Web 應用程式 Proxy （也就是遠端存取服務器角色的角色服務）的詳細資訊，請參閱<a href="https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server">Windows server 2016 中的 Web 應用程式 proxy</a> 。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture">Windows 容器網路功能</a></h3>
                        <hr />
                        <p>Windows 容器網路功能可讓您使用業界標準工具和工作流程，來建立和管理用於連線 Windows 10 和 Windows Server 主機上容器端點的網路。 Windows 容器網路支援多拓撲，包括私人、flat-L2 和 routed-L3。</p>
                        <p>此外，您可以透過與 Windows 主機網路服務（HNS）通訊的外掛程式，使用 Docker、Kubernetes 或 Windows PowerShell，在主機本機上建立重迭。 您可以透過本機代理程式與每個節點的 HNS 進行通訊，藉此建立和管理多節點叢集網路。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/vpn/vpn-top.md">虛擬私人網路 (VPN)</a></h3>
                        <hr />
                        <p>DirectAccess 和 VPN 是 Remote Accessserver 角色的角色服務。</p>
                        <p>當您將遠端存取安裝為 VPN 伺服器時，您可以使用虛擬私人網路（VPN）讓您的遠端員工透過網際網路連線到您的組織網路，同時以加密連接來維護資訊隱私權.</p>
                        <p> 運用 Windows Server 遠端存取 VPN (以及 Windows 10 用戶端電腦)，您可以部署 Always On VPN。 Always On VPN 能讓您管理永遠保持連線的遠端 VPN 用戶端，同時也方便遠端工作者，讓他們不再需要手動連線和中斷連線您組織網路的 VPN。</p>
                        <p>如需詳細資訊，請參閱<a href="https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy">適用于 Windows Server 2016 和 windows 10 的遠端存取 ALWAYS ON VPN 部署指南</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="additional-resources"></a>其他資源

您可以在下列位置取得 Windows Server 2016 之前的作業系統網路資源。

- Windows Server 2012 和 Windows Server 2012 R2 [網路功能概觀](https://technet.microsoft.com/library/hh831357.aspx)
- Windows Server 2008 和 Windows Server 2008 R2 [網路功能](https://technet.microsoft.com/library/cc753940)
- Windows Server 2003 [Windows server 2003/2003 R2 已淘汰的內容](https://www.microsoft.com/download/details.aspx?id=53314)
