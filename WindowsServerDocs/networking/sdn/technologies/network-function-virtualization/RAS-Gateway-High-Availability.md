---
title: RAS 閘道高可用性
description: 您可以使用本主題來瞭解 Windows Server 2019 和2016中軟體定義網路 (SDN) 的「RAS 多租使用者閘道的高可用性設定」。
manager: grcusanz
ms.topic: how-to
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: e591404d4225ff62fc70440a08445255dfe24d6e
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98717014"
---
# <a name="ras-gateway-high-availability"></a>RAS 閘道高可用性

>適用於：Windows Server 2019、Windows Server 2016

您可以使用本主題來瞭解 RAS 多租使用者閘道的高可用性設定，以瞭解軟體定義網路 (SDN) 的高可用性設定。

本主題包含下列各節。

-   [RAS 閘道總覽](#bkmk_overview)

-   [閘道集區總覽](#bkmk_pools)

-   [RAS 閘道部署總覽](#bkmk_deployment)

-   [RAS 閘道與網路控制站整合](#bkmk_integration)

## <a name="ras-gateway-overview"></a><a name="bkmk_overview"></a>RAS 閘道總覽
如果您的組織是雲端服務提供者 (CSP) 或具有多個租使用者的企業，您可以在多租使用者模式中部署 RAS 閘道，以提供進出虛擬和實體網路（包括網際網路）的網路流量。

您可以在多租使用者模式中部署 RAS 閘道作為邊緣閘道，以將租使用者客戶網路流量路由傳送至租使用者虛擬網路和資源。

當您部署多個可提供高可用性和容錯移轉的 RAS 閘道 Vm 實例時，您正在部署閘道集區。 在 Windows Server 2012 R2 中，所有的閘道 Vm 都形成單一集區，讓閘道部署的邏輯分離變得很困難。  Windows Server 2012 R2 閘道為閘道 Vm 提供1:1 個冗余部署，導致無法利用站對站 (S2S) VPN 連線的可用容量。

此問題已在 Windows Server 2016 中解決，它提供多個閘道集區，這可以是不同類型的邏輯分隔。 M + N 冗余的新模式允許更有效率的容錯移轉設定。

如需 RAS 閘道的詳細總覽資訊，請參閱 [Ras 閘道](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。

## <a name="gateway-pools-overview"></a><a name="bkmk_pools"></a>閘道集區總覽
在 Windows Server 2016 中，您可以在一或多個集區中部署閘道。

下圖顯示不同類型的閘道集區，可提供虛擬網路之間的流量路由。

![RAS 閘道集區](../../../media/RAS-Gateway-High-Availability/ras_pools.png)

每個集區都有下列屬性。

-   每個集區都是 M + N 多餘的。 這表示，使用中閘道 vm 的數目是由 ' N ' 個待命閘道 Vm 所備份。 N (待命閘道) 的值一律小於或等於 M (作用中閘道) 。

-   集區可以執行任何個別的閘道函式-網際網路金鑰交換第2版 (IKEv2) S2S、第3層 (L3) ，以及 (GRE) 的一般路由封裝，或集區可執行所有這些功能。

-   您可以將單一公用 IP 位址指派給所有集區，或指派給集區的子集。 這麼做可大幅減少您必須使用的公用 IP 位址數目，因為所有租使用者都可以在單一 IP 位址上連接到雲端。 下一節的高可用性和負載平衡說明其運作方式。

-   您可以藉由新增或移除集區中的閘道 Vm，輕鬆地相應增加或減少閘道集區。 移除或新增閘道不會中斷集區所提供的服務。 您也可以新增和移除整個閘道集區。

-   單一租使用者的連線可以在集區中的多個集區和多個閘道上終止。 但是，如果租使用者的連線在 **所有** 類型閘道集區中終止，它就無法訂閱其他 **所有** 類型或個別類型閘道集區。

閘道集區也提供啟用其他案例的彈性：

-   單一租使用者集區-您可以建立一個集區供一個租使用者使用。

-   如果您透過合作夥伴 (轉銷商) 頻道來銷售雲端服務，您可以為每個轉售商建立個別的集區集。

-   多個集區可以提供相同的閘道功能，但有不同的容量。 例如，您可以建立支援高輸送量和低輸送量 IKEv2 S2S 連接的閘道集區。

## <a name="ras-gateway-deployment-overview"></a><a name="bkmk_deployment"></a>RAS 閘道部署總覽
下圖示范 (CSP) 部署 RAS 閘道的典型雲端服務提供者。

![RAS 閘道部署總覽](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)

使用這種類型的部署時，閘道集區會部署在軟體 Load Balancer (SLB) 上，這可讓 CSP 為整個部署指派單一公用 IP 位址。 租使用者的多個閘道連線可以在多個閘道集區上終止，也可以在集區內的多個閘道上終止。 這會透過上述圖表中的 IKEv2 S2S 連線來說明，但同樣也適用于其他閘道功能，例如 L3 和 GRE 閘道。

在圖例中，MT BGP 裝置是具有 BGP 的 RAS 多租使用者閘道。 動態路由使用多租使用者 BGP。 租使用者的路由是集中式-稱為路由反映 (RR) 的單一點，可處理所有租使用者網站的 BGP 對等互連。 RR 本身會分散到集區中的所有閘道。 這會導致設定，其中租使用者 (資料路徑) 在多個閘道上終止，但租使用者 (BGP 對等互連點控制路徑) 的 RR 只在其中一個閘道。

BGP 路由器會在圖表中分開，以描繪出此集中式路由的概念。 閘道 BGP 實行也提供傳輸路由，可讓雲端作為兩個租使用者網站之間路由傳送的傳輸點。 這些 BGP 功能適用于所有閘道功能。

## <a name="ras-gateway-integration-with-network-controller"></a><a name="bkmk_integration"></a>RAS 閘道與網路控制站整合
RAS 閘道已與 Windows Server 2016 中的網路控制器完全整合。 當您部署 RAS 閘道和網路控制站時，網路控制站會執行下列功能。

-   閘道集區的部署

-   在每個閘道上設定租使用者連接

-   當閘道失敗時，將網路流量切換到待命閘道

下列各節提供有關 RAS 閘道和網路控制站的詳細資訊。

-   [閘道連線 (IKEv2、L3 和 GRE) 的布建和負載平衡 ](#bkmk_provisioning)

-   [IKEv2 S2S 的高可用性](#bkmk_ike)

-   [GRE 的高可用性](#bkmk_gre)

-   [L3 轉送閘道的高可用性](#bkmk_l3)

### <a name="provisioning-and-load-balancing-of-gateway-connections-ikev2-l3-and-gre"></a><a name="bkmk_provisioning"></a>閘道連線 (IKEv2、L3 和 GRE) 的布建和負載平衡
當租使用者要求閘道連線時，會將要求傳送至網路控制站。 網路控制站會設定所有閘道集區的相關資訊，包括每個集區的容量，以及每個集區中的每個閘道。 網路控制站會選取正確的連接集區和閘道。 此選項是以連接的頻寬需求為基礎。 網路控制站使用「最適合」的演算法，在集區中有效率地挑選連接。 如果這是租使用者的第一個連線，這次也會指定連線的 BGP 對等互連點。

在網路控制站選取連線的 RAS 閘道之後，網路控制站會在閘道上布建連線所需的設定。 如果連接是 IKEv2 S2S 連線，網路控制站也會在 SLB 集區上 (NAT) 規則中布建網路位址轉譯;SLB 集區上的此 NAT 規則會將來自租使用者的連線要求導向至指定的閘道。 租使用者會依來源 IP 來區分，這應該是唯一的。

> [!NOTE]
> L3 和 GRE 連線會略過 SLB，並直接連接指定的 RAS 閘道。  這些連線需要遠端端點路由器 (或其他協力廠商裝置) 必須正確地設定，才能與 RAS 閘道連接。

如果已針對連線啟用 BGP 路由，則會由 RAS 閘道起始 BGP 對等互連，並在內部部署與雲端閘道之間交換路由。 如果未使用 BGP，則 BGP (或靜態設定路由的路由會傳送至網路控制器) 。 然後，網路控制站會將路由連接至已安裝租使用者 Vm 的 Hyper-v 主機。 此時，租使用者流量可以路由傳送到正確的內部部署網站。 網路控制站也會建立相關聯的 Hyper-v 網路虛擬化原則來指定閘道位置，並向下連接至 Hyper-v 主機。

### <a name="high-availability-for-ikev2-s2s"></a><a name="bkmk_ike"></a>IKEv2 S2S 的高可用性
集區中的 RAS 閘道包含不同租使用者的連線和 BGP 對等互連。 每個集區都有「作用中閘道」和「N」待命閘道。

網路控制站會以下列方式處理閘道失敗。

-   網路控制站會不斷地 ping 所有集區中的閘道，而且可以偵測到失敗或失敗的閘道。 網路控制站可以偵測到下列類型的 RAS 閘道失敗。

    -   RAS 閘道 VM 失敗

    -   RAS 閘道執行所在的 Hyper-v 主機失敗

    -   RAS 閘道服務失敗

    網路控制站會儲存所有已部署之作用中閘道的設定。 設定包含連接設定和路由設定。

-   當閘道失敗時，它會影響閘道上的租使用者連線，以及位於其他閘道上但其 RR 位於失敗閘道上的租使用者連接。 後次連接的停機時間小於前者。 當網路控制器偵測到失敗的閘道時，它會執行下列工作。

    -   從計算主機移除受影響連接的路由。

    -   移除這些主機上的 Hyper-v 網路虛擬化原則。

    -   選取待命閘道、將其轉換為作用中的閘道，並設定閘道。

    -   變更 SLB 集區上的 NAT 對應，以指向新閘道的連接。

-   同時，隨著新的作用中閘道的設定出現，也會重新建立 IKEv2 S2S 連線和 BGP 對等互連。 連接和 BGP 對等互連可由雲端閘道或內部部署閘道起始。 閘道會重新整理其路由，並將其傳送至網路控制站。 在網路控制站學習閘道探索到的新路由之後，網路控制站會將路由和相關聯的 Hyper-v 網路虛擬化原則傳送至 Hyper-v 主機，也就是失敗影響的租使用者所在的 Vm。 此網路控制卡活動類似于新連線設定的情況，只會發生在較大的規模。

### <a name="high-availability-for-gre"></a><a name="bkmk_gre"></a>GRE 的高可用性
網路控制站的 RAS 閘道容錯移轉回應的程式（包括失敗偵測、將連線和路由設定複製到待命閘道）、對受影響連線的 BGP/靜態路由進行容錯移轉 (包括在計算) 主機上提款和重新處理路由，以及重新設定計算主機上的 Hyper-v 網路虛擬化原則，與 GRE 閘道和連線相同。 重新建立 GRE 連接的方式不同，但 GRE 的高可用性解決方案有一些額外的需求。

![GRE 的高可用性](../../../media/RAS-Gateway-High-Availability/ras_ha.png)

部署閘道時，會將動態 IP 位址指派給每個 RAS 閘道 VM， (DIP) 。 此外，也會將虛擬 IP 位址指派給每個閘道 VM (VIP) 以進行 GRE 高可用性。 Vip 只會指派給可接受 GRE 連線的集區中的閘道，而不會指派給非 GRE 集區。 指派的 Vip 會公告至機架頂端 (使用 BGP TOR) 交換器，然後將 Vip 進一步通告到雲端實體網路。 這可讓閘道從 GRE 連接的另一端所在的遠端路由器或協力廠商裝置連線。 此 BGP 對等互連與租使用者路由交換的租使用者層級 BGP 對等互連不同。

在進行 GRE 連線布建時，網路控制站會選取閘道、在選取的閘道上設定 GRE 端點，並傳回所指派閘道的 VIP 位址。 然後此 VIP 會設定為遠端路由器上的目的地 GRE 通道位址。

當閘道失敗時，網路控制站會將失敗的閘道和其他設定資料的 VIP 位址複製到待命閘道。 當待命閘道變成作用中時，它會將 VIP 通告至其 TOR 交換器，並進一步進入實體網路。 遠端路由器會繼續將 GRE 通道連線至相同的 VIP，路由基礎結構可確保封包會路由傳送至新的使用中閘道。

### <a name="high-availability-for-l3-forwarding-gateways"></a><a name="bkmk_l3"></a>L3 轉送閘道的高可用性
Hyper-v 網路虛擬 L3 轉送閘道是資料中心內的實體基礎結構與 Hyper-v 網路虛擬化雲端中虛擬化基礎結構之間的橋樑。 在多租使用者 L3 轉送閘道上，每個租使用者都會使用自己的 VLAN 標記邏輯網路來與租使用者的實體網路連線。

當新的租使用者建立新的 L3 閘道時，網路控制站閘道 Service Manager 會選取可用的閘道 VM，並使用高可用性的客戶位址來設定新的租使用者介面 (CA) 空間 IP 位址 (從租使用者的 VLAN 標記的邏輯網路) 。 IP 位址會用來做為遠端 (實體網路) 閘道的對等 IP 位址，且是到達租使用者之 Hyper-v 網路虛擬化網路的下一個躍點。

不同于 IPsec 或 GRE 網路連線，TOR 參數將不會動態學習租使用者的 VLAN 標記網路。 租使用者的 VLAN 標記網路的路由必須設定在 TOR 交換器上，以及實體基礎結構與閘道之間的所有中繼交換器和路由器，以確保端對端連線能力。  以下是範例 CSP 虛擬網路設定，如下圖所示。

|網路|子網路|VLAN 識別碼|預設閘道|
|-----------|----------|-----------|-------------------|
|Contoso L3 邏輯網路|10.127.134.0/24|1001|10.127.134.1|
|Woodgrove L3 邏輯網路|10.127.134.0/24|1002|10.127.134.1|

以下是範例租使用者閘道設定，如下圖所示。

|租用戶名稱|L3 閘道 IP 位址|VLAN 識別碼|對等 IP 位址|
|---------------|-------------------------|-----------|-------------------|
|Contoso|10.127.134.50|1001|10.127.134.55|
|Woodgrove|10.127.134.60|1002|10.127.134.65|

以下是 CSP 資料中心內這些設定的圖例。

![L3 轉送閘道的高可用性](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)

L3 轉送閘道內容中的閘道失敗、失敗偵測和閘道容錯移轉程式，類似于 IKEv2 和 GRE RAS 閘道的處理常式。 不同之處在于外部 IP 位址的處理方式。

當閘道 VM 狀態變成狀況不良時，網路控制站會從集區中選取其中一個待命閘道，然後重新布建待命閘道上的網路連線和路由。 移動連線時，L3 轉送閘道的高可用性 CA 空間 IP 位址也會隨著租使用者的 CA 空間 BGP IP 位址移至新的閘道 VM。

因為 L3 對等互連 IP 位址會在容錯移轉期間移至新的閘道 VM，所以遠端實體基礎結構會再次能夠連線到此 IP 位址，並接著連接到 Hyper-v 網路虛擬化工作負載。 針對 BGP 動態路由，因為 CA 空間 BGP IP 位址會移至新的閘道 VM，所以遠端 BGP 路由器可以重新建立對等互連，並再次瞭解所有的 Hyper-v 網路虛擬化路由。

> [!NOTE]
> 您必須個別設定 TOR 交換器和所有中繼路由器，才能使用 VLAN 標記的邏輯網路進行租使用者通訊。 此外，L3 容錯移轉僅限於以這種方式設定的機架。 因此，必須謹慎設定 L3 閘道集區，而且必須另外完成手動設定。



