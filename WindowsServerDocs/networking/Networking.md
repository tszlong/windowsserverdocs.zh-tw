---
title: 網路功能
description: 本主題提供 Windows Server 2016 中可用的「軟體定義網路」和「網路平台」技術概觀。
ms.prod: windows-server-threshold
layout: LandingPage
ms.technology: networking
ms.topic: landing-page
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 09c98d1bd7d2caa8e4cfaea68f9875b25da94003
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444651"
---
# <a name="networking"></a>網路功能

>[!TIP]
> 尋找舊版 Windows Server 的相關資訊嗎？ 查看我們其他位於 docs.microsoft.com 的 [Windows Server 文件庫](/previous-versions/windows/)。 您也可以[搜尋這個網站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)以取得特定資訊。

<HR />

網路功能是軟體定義資料中心的一個基本組件\(SDDC\)平台和 Windows Server 2016 提供全新和改進的軟體定義網路\(SDN\)技術，可協助您移至您的組織的完全實現的 SDDC 解決方案。

當您將網路當成軟體定義的資源來管理時，您可以說明應用程式的基礎結構需求一次，然後選擇應用程式執行的位置 (內部部署或在雲端中)。 

這種一致性表示您的應用程式現在能夠輕易地進行擴充，而您可以隨時隨地順暢地執行應用程式，並在安全性、效能、服務品質及可用性方面具有同等的信心。

<HR />

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
                                        <h2><a href="../networking/What-s-New-in-Networking.md">什麼&#39;的新網路功能</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

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
                                        <h3><a href="https://docs.microsoft.com/windows-server/networking/sdn/">軟體定義網路 (SDN)</a><hr /></h3>您可以使用本主題來了解 Windows Server、System Center 和 Microsoft Azure 中提供的 SDN 技術。</p>
                        
                                        <p><b>注意：</b> 對於 HYPER-V 主機並執行 SDN 基礎結構伺服器，例如網路控制站和軟體負載平衡節點的虛擬機器 (Vm) 中，您必須安裝 Windows Server Datacenter edition。 對於包含唯一的租用戶工作負載會連接到 SDN 控制網路的 Vm 的 HYPER-V 主機，您可以執行 Windows Server Standard edition。</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md">部署軟體定義網路基礎結構使用指令碼</a><hr /></h3>本指南提供如何在測試實驗室環境中，使用虛擬網路和閘道來部署網路控制站的相關指示。</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-controller/Network-Controller.md">網路控制卡</a><hr /></h3>網路控制站提供集中式、可程式化的自動化點，可以管理、設定、監視和疑難排解資料中心內的虛擬和實體網路基礎結構。</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md">軟體負載平衡&#40;SLB&#41;適用於 SDN</a><hr /></h3>雲端服務提供者 (Csp) 和要部署軟體定義網路 (SDN) Windows Server 2016 中的企業可以使用軟體負載平衡 (SLB) 將租用戶和租用戶客戶網路流量在虛擬網路資源之間平均地分散。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md">適用於 SDN 的 RAS 閘道</a><hr /></h3>RAS 閘道，也就是以軟體為基礎，多租用戶、 邊界閘道通訊協定 (BGP) 功能的路由器，在 Windows Server 2016 中，被專為雲端服務提供者 (Csp) 和裝載使用 HYPER-V 網路的多個租用戶虛擬網路的企業虛擬化。</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md">網路功能虛擬化</a><hr /></h3>在軟體定義的資料中心，會逐漸正在要由硬體設備 （例如負載平衡器、 防火牆、 路由器、 交換器等等） 的網路功能虛擬化為虛擬設備。 這&quot;網路功能虛擬化&quot;是伺服器虛擬化和網路虛擬化的自然進展。</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md">資料中心防火牆概觀</a><hr /></h3>資料中心防火牆是一個網路層、5-Tuple (通訊協定、來源和目的地連接埠號碼，以及來源和目的地 IP 位址)、可設定狀態、多租用戶的防火牆。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<HR />

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
                                        <h3><a href="branchcache/BranchCache.md">BranchCache</a><hr /></h3>
                                        <p>BranchCache 是廣域網路 (WAN) 頻寬最佳化技術。 為了在使用者存取遠端伺服器的內容時將 WAN 頻寬最佳化，BranchCache 會從總公司或託管的雲端內容伺服器擷取內容，並在分公司快取內容，讓分公司的用戶端電腦可從本機存取內容而非透過 WAN。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="core-network-guide/core-network-guide-windows-server.md">核心網路指南</a><hr /></h3>
                                        <p>了解如何利用「核心網路指南」部署 Windows Server 網路，以及利用「核心網路附屬指南」將功能新增至您的網路部署。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../remote/remote-access/directaccess/DirectAccess.md">DirectAccess</a><hr /></h3>
                                        <p>DirectAccess 可允許遠端使用者連線至組織網路資源。 </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="dns/dns-top.md">網域名稱系統 (DNS)&quot;&gt;</a><hr /></h3>
                                        <p>網域名稱系統 (DNS) 是其中一個構成 TCP/IP 通訊協定的業界標準套件，並在一起的 DNS 用戶端和 DNS 伺服器提供 電腦名稱到 IP 位址對應名稱解析服務的電腦和使用者。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/dhcp/dhcp-top.md">動態主機設定通訊協定&#40;DHCP&#41;</a><hr /></h3>
                                        <p>動態主機設定通訊協定 (DHCP) 是一種用戶端/伺服器通訊協定，會自動提供網際網路通訊協定 (IP) 主機，其 IP 位址及其他相關的組態資訊，例如子網路遮罩及預設閘道。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md">HYPER-V 網路虛擬化</a><hr /></h3>
                                        <p>HYPER-V 網路虛擬化 (HNV) 可讓您共用的實體網路基礎結構之上將客戶網路虛擬化。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">Hyper-V 虛擬交換器</a><hr /></h3>
                                        <p>Hyper-V 虛擬交換器是一種軟體式 Layer-2 乙太網路交換器，安裝 Hyper-V 伺服器角色時會隨附在 Hyper-V 管理員中。 交換器包含以程式設計方式管理和可擴充的功能，將虛擬機器連線到虛擬網路和實體網路。 此外，Hyper-V 虛擬交換器提供安全性、隔離以及服務層級的原則強化。 </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/ipam/ipam-top.md">IP 位址管理&#40;IPAM&#41;</a><hr /></h3>
                                        <p>IP 位址管理 (IPAM) 是一個整合的工具，可啟用端對端規劃、 部署、 管理和監視 IP 位址基礎結構，具有豐富的使用者體驗的套件。 IPAM 會自動探索您網路上的 IP 位址基礎結構伺服器和網域名稱系統 (DNS) 伺服器，並可讓您從中央介面管理這些伺服器。 </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/Network-Load-Balancing.md">網路負載平衡</a><hr /></h3>
                                        <p>網路負載平衡 (NLB) 會將流量分散到數部伺服器使用 TCP/IP 網路通訊協定。 針對非 SDN 部署，NLB 可確保無狀態的應用程式，例如執行 Internet Information Services (IIS) 網頁伺服器是可調整，新增更多的伺服器，在負載增加時。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/hpn/hpn-top.md">高效能的網路功能</a><hr /></h3>
                                        <p>Windows Server 2016 中的網路卸載和最佳化技術包含「僅限軟體」(SO) 功能和技術、「硬體和軟體」(SH) 整合功能和技術，以及「僅限硬體」(HO) 功能和技術。</p>

                                        <p>此外也提供下列卸載和最佳化技術文件：<p>
                                        <hr />
                                        <a href="technologies/conv-nic/cnic-top.md">高效能的網路功能</a><hr />
                                        <a href="technologies/dcb/dcb-top.md">資料中心橋接 (DCB)</a><hr />
                                        <a href="technologies/vrss/vrss-top.md">虛擬接收端調整 (vRSS)</a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/nps/nps-top.md">網路原則伺服器</a><hr /></h3>
                                        <p>
網路原則伺服器 (NPS) 可讓您建立並執行全組織網路存取原則，以用於連線要求驗證與授權。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/netsh/netsh.md">網路殼層 (Netsh)</a><hr /></h3>
                                        <p>
您可以使用網路殼層 (netsh) 網路公用程式來管理 Windows Server 2016 和 Windows 10 中的網路技術。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/network-subsystem/net-sub-performance-top.md">網路子系統效能調整</a><hr /></h3>
                                        <p>
本主題提供有關選擇正確的網路介面卡，為您的伺服器工作負載，排序網路介面、 網路相關效能計數器以及效能調整網路介面卡和相關的網路技術，例如接收端調整 (RSS)、 接收端聯合 (RSC)，和其他項目。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/nic-teaming/NIC-Teaming.md">NIC 小組</a><hr /></h3>
                                        <p>
NIC 小組可讓您將實體的乙太網路介面卡群組為一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/qos/qos-policy-top.md">品質的服務 (QoS) 原則</a><hr /></h3>
                                        <p>
您可以透過建立 QoS 設定檔並使用群組原則發佈其設定，來使用 QoS 原則做為整體 Active Directory 基礎結構的網路頻寬管理中心點。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/wins/wins-top.md">Windows 網際網路名稱服務 (WINS)</a><hr /></h3>
                                        <p>
Windows 網際網路名稱服務 (WINS) 是舊版電腦名稱登錄與解析服務，可將電腦 NetBIOS 名稱對應至 IP 位址。 建議使用 DNS 而不要使用 WINS。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../remote/remote-access/remote-access.md">遠端存取</a><hr /></h3>
                                        <p>
您可以使用遠端存取技術，例如 DirectAccess 和虛擬私人網路 (VPN)，若要讓遠端工作者連線到內部網路資源。 此外，您可以使用遠端存取區域網路 (LAN) 路由，以及 Web 應用程式 Proxy。 這可為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以允許任何裝置上的使用者從公司網路外部存取這些應用程式。</p>

                                        <p>如需有關 Web 應用程式 Proxy，也就是遠端存取伺服器角色的角色服務，請參閱<a href="https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server">Windows Server 2016 中的 Web 應用程式 Proxy</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture">Windows 容器網路功能</a><hr /></h3>
                                        <p>
Windows 容器網路功能可讓您使用業界標準工具和工作流程，來建立和管理用於連線 Windows 10 和 Windows Server 主機上容器端點的網路。 Windows 容器網路支援多拓撲，包括私人、flat-L2 和 routed-L3。</p>

                                        <p>也支援是重疊，您可以建立在本機主機上使用 Docker、 Kubernetes 或 Windows PowerShell，透過通訊與 Windows 主機網路服務 (HNS) 的外掛程式。 您可以建立及管理透過更高的層級的協調流程系統的多節點叢集網路通訊透過每個節點的 HNS 本機代理程式。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../remote/remote-access/vpn/vpn-top.md">虛擬私人網路 (VPN)</a><hr /></h3>
                                        <p>
DirectAccess 和 VPN 是 Remote Accessserver 角色的角色服務。</p>

                                        <p>當您安裝遠端存取 VPN 伺服器時，您可以使用虛擬私人網路 (VPN) 提供您連線的遠端員工對您的組織網路網際網路-同時維持與加密的連線資訊隱私權.</p>

                                       <p> 運用 Windows Server 遠端存取 VPN (以及 Windows 10 用戶端電腦)，您可以部署 Always On VPN。 Always On VPN 能讓您管理永遠保持連線的遠端 VPN 用戶端，同時也方便遠端工作者，讓他們不再需要手動連線和中斷連線您組織網路的 VPN。</p>

                                       <p>如需詳細資訊，請參閱<a href="https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy">遠端存取一律在 VPN 部署指南的 Windows Server 2016 和 Windows 10</a></p>
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
- Windows Server 2003 [Windows Server 2003/2003 R2 退場內容](https://www.microsoft.com/download/details.aspx?id=53314)
