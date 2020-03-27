---
title: 邊界閘道通訊協定 (BGP)
description: 您可以使用本主題來瞭解 Windows Server 2016 中的邊界閘道協定（BGP），包括 BGP 支援的部署拓撲和 BGP 特性和功能。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78cc2ce3-a48e-45db-b402-e480b493fab1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3e04c732cbacb182731717215a4cf99cf3cc1f76
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309295"
---
# <a name="border-gateway-protocol-bgp"></a>邊界閘道通訊協定 (BGP)

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來了解邊界閘道通訊協定 (BGP)，包括 BGP 支援的部署拓撲和 BGP 特性與功能。  
  
> [!NOTE]  
> 除了本主題之外，還有下列 BGP 檔可供使用。  
>   
> -   [BGP Windows PowerShell 命令參考](../../remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
本主題包含下列各節。  
  
-   [BGP 支援的部署拓撲](#bkmk_top)  
  
-   [BGP 功能](#bkmk_features)  
  
在 Windows Server 2016 遠端存取服務上設定時，在多租使用者模式中 \(RAS\) 閘道，邊界閘道協定（BGP）可讓您管理租使用者 VM 網路和其遠端網站之間的網路流量路由。 您也可以針對單一租使用者 RAS 閘道部署使用 BGP，並將遠端存取部署為區域網路 \(LAN\) 路由器。  
  
BGP 可以降低在路由器上手動路由設定的需求，因為它是動態路由通訊協定，並且會自動學習使用站台對站台 VPN 連線來連接的網站之間的路由。  
  
若要使用 BGP 路由，您必須在電腦或虛擬機器 \(VM 上的遠端存取服務器角色 \(RAS\)和/或**路由**角色服務安裝**遠端存取服務**\)-您使用的系統類型取決於您是否有多租使用者部署：  
  
-   針對多租使用者部署，建議您在一或多個 Vm 上安裝 RAS 閘道。 使用多個 Vm 可提供高可用性。 RAS 閘道能夠處理來自多個租使用者的多個連線，並且是由 Hyper-v 主機和實際設定為閘道的 VM 所組成。 此閘道設定了站對站 VPN 連線作為多租使用者 BGP 路由器，以 \(CSP\) 子網路由。  
  
-   針對單一租使用者邊緣閘道部署或 LAN 路由器部署，您可以在實體電腦或 VM 上安裝 RAS 閘道。  
  
> [!IMPORTANT]  
> 當您安裝 RAS 閘道時，您必須使用**Enable-remoteaccessroutingdomain** Windows PowerShell 命令，並將**類型**參數值為**All**，指定是否要為每個租使用者啟用 BGP。 若要將遠端存取安裝為已啟用 BGP 的 LAN 路由器，而不使用多租使用者功能，您可以使用命令**install-RemoteAccess-VpnType RoutingOnly**。  
>   
> 下列範例程式碼說明如何在多租使用者模式中安裝 RAS，其中包含兩個租使用者 Contoso 和 Fabrikam 的所有 RAS 功能（點對站 VPN、站對站 VPN 和 BGP 路由）。  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
## <a name="bgp-supported-deployment-topologies"></a><a name="bkmk_top"></a>BGP 支援的部署拓撲  
以下列出企業網站連接到雲端服務提供者 (CSP) 的資料中心支援的部署拓撲。  
  
在所有案例中，CSP 閘道是邊緣的 Windows Server 2016 RAS 閘道。 RAS 閘道能夠處理來自多個租使用者的多個連線，這是由 Hyper-v 主機和實際設定為閘道的 VM 所組成。 此邊緣閘道是設定為使用站台對站台 VPN 連線做為多租用戶 BGP 路由器，以交換企業和 CSP 子網路路由。  
  
租用戶使用站台對站台 (S2S) VPN 連線連接到其在 CSP 資料中心的資源。 此外，部署 BGP 路由通訊協定以用於企業和 CSP 閘道之間的動態路由資訊交換。  
  
支援下列部署拓撲。  
  
-   [在企業網站邊緣具有 BGP 的 RAS VPN 站對站閘道](#bkmk_top1)  
  
-   [在企業網站邊緣具有 BGP 的協力廠商閘道](#bkmk_top2)  
  
-   [具有協力廠商閘道的多個企業網站](#bkmk_top3)  
  
-   [BGP 和 VPN 的個別終止點](#bkmk_top4)  
  
下列各節包含每個支援的 BGP 拓撲的其他資訊。  
  
### <a name="ras-vpn-site-to-site-gateway-with-bgp-at-enterprise-site-edge"></a><a name="bkmk_top1"></a>在企業網站邊緣具有 BGP 的 RAS VPN 站對站閘道  
此拓撲描述連接至 CSP 的企業網站。 企業路由拓撲包含內部路由器、針對與 CSP 的 VPN 站對站連線設定的 Windows Server 2016 RAS 閘道，以及邊緣防火牆裝置。 RAS 閘道會終止 S2S VPN 和 BGP 連線。  
  
![RAS VPN 站對站閘道](../../media/Border-Gateway-Protocol-BGP/bgp_01.jpg)  
  
這兩個網站使用外部邊界閘道通訊協定 (eBGP) 連接，可以在不同自發的系統 (AS) 中啟用 BGP 的路由器之間傳輸資訊。 這需要企業和 CSP 具有相異自發的系統編號 (ASN)，這是 BGP 通訊協定不可或缺的參數。  
  
在這個案例中，BGP 適用以下方式。  
  
-   企業網站邊緣裝置會學習使用 BGP 裝載在雲端的虛擬化子網路路由 (10.2.1.0/24)。 此裝置也會向 CSP RAS 多租使用者閘道通告內部部署子網路由（10.1.1.0/24）。  
  
-   客戶邊緣路由器可透過下列其中一個機制學習內部部署內部路由：  
  
    -   邊緣裝置會使用內部路由器執行 BGP，並學習內部路由 (在此範例中為 10.1.1.0/24)。 同時，內部路由器會從邊緣裝置學習外部路由 (例如 10.2.1.0/24)，而內部路由器必須使用內部閘道通訊協定 (IGP) (例如先開啟最短的路徑 (OSPF) 或路由資訊通訊協定 (RIP))，散發這些路由到其他內部部署路由器。  
  
    -   邊緣裝置可以使用 BGP 設定為使用靜態路由或介面以選取通告的路由。 邊緣裝置也會將外部路由發佈至使用 IGP 的其他內部部署路由器。  
  
### <a name="third-party-gateway-with-bgp-at-enterprise-site-edge"></a><a name="bkmk_top2"></a>在企業網站邊緣具有 BGP 的協力廠商閘道  
此拓撲描述使用協力廠商邊緣路由器連線至 CSP 的企業網站。 邊緣路由器也可做為站台對站台 VPN 閘道。  
  
![協力廠商閘道與企業網站邊緣的 BGP](../../media/Border-Gateway-Protocol-BGP/bgp_02.jpg)  
  
企業邊緣路由器可透過下列其中一個機制學習內部部署內部路由：  
  
-   邊緣裝置會使用內部路由器執行 BGP，並學習內部路由 (在此情況下，10.1.1.0/24)  
  
-   邊緣裝置會實作內部閘道通訊協定 (IGP)，並直接參與內部路由。  
  
### <a name="multiple-enterprise-sites-connecting-to-csp-cloud-datacenter"></a><a name="bkmk_top3"></a>連接至 CSP 雲端資料中心的多個企業網站  
此拓撲描述使用協力廠商閘道連線至 CSP 的多個企業網站。 協力廠商邊緣裝置做為站台對站台 VPN 閘道和 BGP 路由器。  
  
![連接至 CSP 雲端資料中心的多個企業網站](../../media/Border-Gateway-Protocol-BGP/bgp_03.jpg)  
  
客戶邊緣路由器可透過下列其中一個機制學習內部部署內部路由：  
  
-   邊緣裝置會使用內部路由器執行 BGP，並學習內部路由 (在此情況下，10.1.1.0/24)  
  
-   邊緣裝置會實作內部閘道通訊協定 (IGP)，並直接參與內部路由。  
  
每個企業網站可透過直接 eBGP 連線學習其他網站的路由。  
  
每個企業網站會學習直接裝載的網路路由，並透過使用其他企業網站，但會根據路由的成本選取最佳的路由。  
  
如果企業網站1的 BGP 路由器無法與企業網站 2 BGP 路由器連線，因為連線已失敗，則 Site 1 BGP 路由器會動態開始學習從 CSP BGP 路由器到 Enterprise Site 2 網路的路由，而流量會順暢地透過 CSP 的 Windows Server BGP 路由器，從網站1重新路由至網站2。  
  
### <a name="separate-termination-points-for-bgp-and-vpn"></a><a name="bkmk_top4"></a>BGP 和 VPN 的個別終止點  
此拓撲描述使用兩個不同的路由器做為 BGP 和站台對站台 VPN 端點的企業。 站對站 VPN 會在 Windows Server 2016 RAS 閘道上終止，而 BGP 則是在內部路由器上終止。 在連線的 CSP 端，CSP 會同時終止與 RAS 閘道的 VPN 和 BGP 連線。 使用這種設定，內部的協力廠商路由器硬體必須支援對 BGP 的 IGP 路由轉散發，以及對 IGP 路由的 BGP 轉散發。  
  
![區隔 BGP 和 VPN 的終止點](../../media/Border-Gateway-Protocol-BGP/bgp_04.jpg)  
  
內部路由器可透過下列其中一個機制學習企業路由：  
  
-   BGP  
  
-   內部閘道通訊協定 (IGP) 例如 OSPF 或 RIP。  
  
-   靜態路由設定  
  
在企業網站使用任何 IGP 時，內部路由器必須轉散發 IGP 路由到 BGP，以及轉散發 BGP 路由到 IGP 路由，用於保有 CSP 虛擬網路與本機企業子網路之間的子網路連線。  
  
在此部署中，企業 RAS 閘道具有與 CSP RAS 閘道的站對站 VPN 連線，可為企業 RAS 閘道提供 CSP 閘道的路由。 然後，企業內部路由器會使用 iBGP 搭配企業 RAS 閘道來學習此路由至 CSP 閘道。 因此，企業內部路由器就能夠與 CSP RAS 閘道 BGP 路由器建立對等互連會話。  
  
從現在開始，企業內部路由器和 CSP RAS 閘道會交換路由資訊。 而企業 RAS BGP 路由器會學習 CSP 路由和企業路由，以在網路之間實際路由封包。  
  
## <a name="bgp-features"></a><a name="bkmk_features"></a>BGP 功能  
以下是 RAS 閘道 BGP 路由器的功能。  
  
**作為遠端存取角色服務的 BGP 路由**。 當您想要使用遠端存取作為 BGP LAN 路由器時，您現在可以安裝遠端存取服務器角色的**路由**角色服務，而不需要安裝**遠端存取服務（RAS）** 角色服務。  這會減少 BGP 路由器記憶體使用量，而且只會安裝動態 BGP 路由所需的元件。 只有當需要 BGP 路由器 VM，而且您不需要使用 DirectAccess 或 VPN 時，路由角色服務才會很有用。 此外，使用「遠端存取」作為具有 BGP 的 LAN 路由器，可提供您內部網路上 BGP 的動態路由優勢。  
  
**BGP 統計資料 (訊息計數器、路由計數器)** 。 如需要，BGP 路由器可支援顯示訊息和路由的統計資料，方法是使用 **Get-BgpStatistics** Windows PowerShell 命令。  
  
**相同成本多路徑路由 (ECMP) 支援**。 BGP 路由器可支援 ECMP，並且可以有多個相同成本的路由連接到 BGP 路由表和堆疊。 若啟用 ECMP，用於傳輸資料封包的路由的 BGP 路由器選取是隨機的。  
  
**HoldTime 設定**。 根據您的網路需求，BGP 路由器支援 HoldTimer 值的設定。 這個計時器可動態方式變更，以容納與協力廠商裝置的互通性或保有 BGP 對等互連工作階段逾時的特定最大時間。  
  
**內部 BGP 和外部 BGP 支援**。 BGP 路由器支援 iBGP 和 eBGP 對等互連。 若要設定任一項，您必須確定指派適當的 ASN 給本機和遠端的 BGP 路由器。 所有四個 BGP 部署拓撲採用 eBGP 對等互連，而第四個拓撲也使用 iBGP 對等互連。  
  
**與協力廠商解決方案之間的互通性**。 BGP 路由器是基於最新的 BGP 第 4 版規格，並經過測試可與大部分主要的協力廠商 BGP 路由裝置互通。 如需詳細資訊，請參閱要求建議 (RFC) 4271 [邊界閘道通訊協定 4 (BGP-4)](https://tools.ietf.org/html/rfc4271)。  
  
**IPv4 和 IPv6 傳輸對等互連支援**。 BGP 路由器支援 IPv4 和 IPv6 對等互連。 不過，您必須將 BGP 識別碼設定為 BGP 路由器的 IPv4 位址。 針對所有 BGP 路由器部署拓撲，可以使用兩個對等互連類型 (IPV4/IPv6) 中的一個。  
  
**IPv4 和 IPv6 單點傳播路由學習和通告功能 (多重通訊協定網路層連線資訊 [NLRI])** 。 不論您使用何種傳輸，在建立工作階段時如果其他 BGP 路由器推出適當的功能，BGP 路由器即可以交換 IPv4 和 IPv6 路由。 若要設定 IPv6 路由，必須啟用參數 IPv6Routing，並且必須在路由器層級設定本機全域 IPv6 位址。  
  
**混合模式與被動模式對等互連**。 您可以在混合模式中設定 BGP 對等互連會話，其中 BGP 路由器會作為啟動器和回應者或被動模式，其中 BGP 路由器不會起始對等互連，但會回應傳入的要求。 混合模式是預設值，並建議用於 BGP 對等互連。 除非您想要將被動模式用於偵錯或診斷用途，否則也是如此。 針對所有 BGP 路由器部署拓撲，需要混合模式對等互連，才能在發生失敗事件時啟用自動重新啟動。  
  
**路由屬性重寫功能**。 您可以從 BGP 路由器輸入和輸出路由通告新增、修改或移除下列屬性，方法是使用 BGP 路由原則 Next-Hop、MED、Local-Pref 和 Community。  
  
**路由篩選**。 BGP 路由器支援根據多個路由屬性 (例如 Prefix、ASN-Range、Community 和 Next-Hop) 篩選輸入或輸出路由通告。  
  
**路由反映器（rr）和 RR 用戶端**。 BGP 路由器可以做為路由反映程式和 RR 用戶端。 這適用于複雜的拓撲，其中 RR 可以藉由形成 RR 叢集來簡化網路。  
  
**路由重新整理支援**。 BGP 路由器支援路由重新整理，並預設會在對等互連通告這項功能。 當對等端透過路由重新整理訊息要求時，它能夠傳送一組全新的路由更新，以及傳送路由重新整理來更新對等的路由策略變更等事件中的路由表。 這可讓您在 Windows Server 2016 中變更或更新 BGP 路由原則的案例，而不需要重新開機對等互連。  
  
**靜態路由設定支援**。 您可以在 BGP 路由器上使用 **Add-BgpCustomRoute** Windows PowerShell 命令來設定靜態路由或介面。 您設定的靜態路由可以是必須從中選擇之路由的前置詞或介面名稱。 但是，只有具有可解析的下一個躍點的路由會連接至 BGP 路由表並通告給對等。  
  
**傳輸路由支援**。 BGP 路由器支援 iBGP 至 iBGP 連線、iBGP 至 eBGP 連線，以及 eBGP 至 eBGP 連線的傳輸路由。  
  
**路由擺動抑制**。 在 Windows Server 2016 中路由擺動抑制至 BGP 路由，可支援路由擺動抑制。 例如，當路由持續被公告和撤銷，使路由表不穩定時，您可以將 BGP 路由器設定為指派抑制權數給路由，並加以監視以進行副總，並視需要予以抑制或取消隱藏。 這有助於維護穩定的路由表，以及 BGP 路由器的較少處理。  
  
**路由匯總**。 將匯總路由至 BGP 路由器可提供您設定匯總路由的能力，並以摘要或匯總路由取代更細微的路由通告給對等。 這會導致在網路上傳輸的路由通告訊息數目較少。  
  
> [!NOTE]  
> 在 System Center 中，RAS 閘道的名稱是 Windows Server Gateway。  
  

  

