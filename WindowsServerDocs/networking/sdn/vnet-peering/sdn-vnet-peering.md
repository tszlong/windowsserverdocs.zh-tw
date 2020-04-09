---
title: 虛擬網路對等互連
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: c5955ad8be987ebfd605bb59cfdc43b9011714c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853581"
---
# <a name="virtual-network-peering"></a>虛擬網路對等互連

>適用于： Windows Server

虛擬網路對等互連可讓您順暢地連接兩個虛擬網路。 一旦對等互連，基於連線目的，虛擬網路會顯示為一。 

使用虛擬網路對等互連的優點包括：

-   對等互連虛擬網路中虛擬機器之間的流量只會透過*私人*IP 位址透過骨幹基礎結構路由傳送。 虛擬網路之間的通訊不需要公用網際網路或閘道。

-   不同虛擬網路中資源之間的低延遲、高頻寬連線。

-   一個虛擬網路中的資源與不同虛擬網路中的資源進行通訊的能力。

-   建立對等互連時，任一虛擬網路中的資源都不會停機。

## <a name="requirements-and-constraints"></a>需求和限制

虛擬網路對等互連有幾個需求和限制：

- 對等互連虛擬網路必須：

  -   具有非重迭的 IP 位址空間

  -   由相同的網路控制卡管理

- 將虛擬網路與另一個虛擬網路對等互連之後，您就無法在位址空間中新增或刪除位址範圍。

  >[!TIP]
  >如果您需要新增位址範圍：<ol><li>移除對等互連。</li><li>新增位址空間。</li><li>再次新增對等互連。</li></ol>

- 由於虛擬網路對等互連是介於兩個虛擬網路之間，因此在對等互連之間沒有任何衍生的可轉移關聯性。 例如，如果您將 virtualNetworkA 與 virtualNetworkB 和 virtualNetworkB 搭配 virtualNetworkC 進行對等互連，則 virtualNetworkA 不會取得對等互連與 virtualNetworkC。

  [這裡的影像]

## <a name="connectivity"></a>連線能力

對等互連虛擬網路之後，任一虛擬網路中的資源都可以直接與對等互連虛擬網路中的資源連線。

-   對等互連虛擬網路中虛擬機器之間的網路延遲與單一虛擬網路中的延遲相同。

-   網路輸送量是以虛擬機器允許的頻寬為基礎。 對等互連中的頻寬沒有任何額外的限制。

-   對等互連虛擬網路中虛擬機器之間的流量會透過骨幹基礎結構直接路由傳送，而不是透過閘道或公用網際網路。

-   虛擬網路中的虛擬機器可以存取對等互連虛擬網路中的內部負載平衡器。

如有需要，您可以在任一虛擬網路中套用存取控制清單（Acl），以封鎖其他虛擬網路或子網的存取。 如果您開啟對等互連虛擬網路（這是預設選項）之間的完整連線，您可以將 Acl 套用至特定子網或虛擬機器，以封鎖或拒絕特定的存取。 若要深入瞭解 Acl，請參閱[使用存取控制清單（acl）來管理資料中心網路流量](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

## <a name="service-chaining"></a>服務鏈

您可以設定使用者定義的路由，以指向對等互連虛擬網路中的虛擬機器做為下一個躍點 IP 位址，以啟用服務鏈。 服務連結可讓您透過使用者定義的路由，將流量從一個虛擬網路導向至對等互連虛擬網路中的虛擬裝置。

您可以部署中樞和輪輻網路，其中中樞虛擬網路可以裝載基礎結構元件，例如網路虛擬裝置。 所有與中樞虛擬網路對等的輪輻虛擬網路。 流量可以流經中樞虛擬網路中的網路虛擬裝置。

虛擬網路對等互連可讓使用者定義路由中的下一個躍點成為對等互連虛擬網路中虛擬機器的 IP 位址。 若要深入瞭解使用者定義的路由，請參閱[在虛擬網路上使用網路虛擬裝置](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn)。

## <a name="gateways-and-on-premises-connectivity"></a>閘道和內部部署連線能力

無論是否有另一個虛擬網路對等互連，每個虛擬網路仍然可以有自己的閘道連接到內部部署網路。 當您對等互連虛擬網路時，您也可以將對等互連虛擬網路中的閘道設定為內部部署網路的傳輸點。 在此情況下，使用遠端閘道的虛擬網路不能有自己的閘道。 虛擬網路只能有一個閘道可以是本機或遠端閘道（在對等互連虛擬網路中）。

## <a name="monitor"></a>監視器

當您對等互連兩個虛擬網路時，必須為對等互連中的每個虛擬網路設定對等互連。

您可以監視對等互連連線的狀態，這可以是下列其中一種狀態：

-   **起始：** 當您建立從第一個虛擬網路到第二個虛擬網路的對等互連時顯示。

-   **已連線：** 在您建立從第二個虛擬網路到第一個虛擬網路的對等互連之後顯示。 第一個虛擬網路的對等互連狀態從 [已起始] 變更為 [已連線]。 在成功建立虛擬網路對等互連之前，這兩個虛擬網路對等的狀態必須是 [已連線]。

-   已**中斷連線：** 當一個虛擬網路與另一個虛擬網路中斷連線時顯示。

[狀態的資訊圖]

## <a name="next-steps"></a>後續步驟
[設定虛擬網路對等互連](sdn-configure-vnet-peering.md)：在此程式中，您會使用 Windows PowerShell 來尋找 HNV 提供者邏輯網路，以建立兩個虛擬網路，每一個都有一個子網。 您也可以設定兩個虛擬網路之間的對等互連。

