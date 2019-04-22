---
title: 虛擬網路對等互連
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 58596387d79f3f212a472f00c2785bacc278e855
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821909"
---
# <a name="virtual-network-peering"></a>虛擬網路對等互連

>適用於：Windows Server

虛擬網路對等互連可讓您順暢地連線兩個虛擬網路。 一旦對等互連，進行連線，虛擬網路會顯示為其中。 

使用虛擬網路對等互連的優點包括：

-   對等互連的虛擬網路中的虛擬機器之間的流量路由傳送透過骨幹基礎結構，透過*私人*IP 位址只。 虛擬網路之間的通訊不需要公用網際網路或閘道。

-   之間不同的虛擬網路中資源的低延遲、 高頻寬連線。

-   能夠在一個虛擬網路的資源與不同的虛擬網路中的資源進行通訊。

-   建立對等互連時，任一虛擬網路中的資源沒有停機時間。

## <a name="requirements-and-constraints"></a>需求和限制

虛擬網路對等互連有幾個需求和限制：

-   對等互連的虛擬網路必須：

    -   有非重疊的 IP 位址空間

    -   由相同的網路控制卡管理

-   一旦您對等互連虛擬網路與另一個虛擬網路，您無法加入或刪除位址空間中的位址範圍。

   >[!TIP]
   >如果您需要新增位址範圍：<ol><li>移除對等互連。</li><li>新增位址空間。</li><li>新增對等互連一次。</li></ol>

-   因為虛擬網路對等互連是介於兩個虛擬網路，則沒有任何衍生的可轉移關聯性儲存跨對等互連上。 比方說，如果您對等互連 virtualNetworkA 與 virtualNetworkB 與 virtualNetworkC virtualNetworkB，則 virtualNetworkA 不取得對等互連與 virtualNetworkC。

    [在此映像]

## <a name="connectivity"></a>連線能力

虛擬網路對等互連後，任一虛擬網路中的資源可以直接連線的對等互連的虛擬網路中的資源。

-   對等互連的虛擬網路中的虛擬機器之間的網路延遲是單一的虛擬網路延遲相同。

-   網路輸送量為依照虛擬機器允許的頻寬。 沒有任何額外的限制內的對等互連的頻寬。

-   對等互連的虛擬網路中的虛擬機器之間的流量會透過骨幹基礎結構，不是透過閘道或透過公用網際網路直接路由。

-   虛擬網路中的虛擬機器可存取內部負載平衡器的對等互連的虛擬網路中。

您可以套用存取控制清單 (Acl)，以封鎖其他虛擬網路或子網路的存取，如有需要的任一虛擬網路中。 如果您開啟對等互連的虛擬網路 （這是預設選項） 之間的完整連線，您可以將 Acl 套用至特定子網路或虛擬機器，以封鎖或拒絕特定存取。 若要深入了解 Acl，請參閱[使用存取控制清單 (Acl) 來管理資料中心網路流量流動](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

## <a name="service-chaining"></a>服務鏈結

您可以設定使用者定義的路由，指向對等互連的虛擬網路中的虛擬機器的下一個躍點 IP 位址，以啟用服務鏈結。 服務鏈結可讓您，將流量從一個虛擬網路虛擬設備，在對等互連虛擬網路中，透過使用者定義的路由。

您可以部署中樞輪輻網路，讓中樞虛擬網路可以裝載基礎結構元件，例如網路虛擬設備。 所有輪輻虛擬網路對等都互連與中樞虛擬網路中。 流量可以通過在中樞虛擬網路中的網路虛擬設備。

虛擬網路對等互連可讓要對等互連的虛擬網路中的虛擬機器的 IP 位址的使用者定義路由中下一個躍點。 若要深入了解使用者定義的路由，請參閱[虛擬網路上使用網路虛擬設備](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn)。

## <a name="gateways-and-on-premises-connectivity"></a>閘道與內部部署連線能力

每個虛擬網路，不論是否與另一個虛擬網路對等互連仍然可以有它自己的閘道器連接至內部部署網路。 當您對等互連虛擬網路時，您也可以到內部部署網路的傳輸點設定中的對等互連的虛擬網路閘道。 在此情況下，使用遠端閘道的虛擬網路不能有自己的閘道。 虛擬網路只能擁有一個閘道，可以是本機或遠端閘道 （在對等互連的虛擬網路中）。

## <a name="monitor"></a>監視器

當您對等互連兩個虛擬網路時，您必須設定每個虛擬網路對等互連的對等互連。

您可以監視您的對等互連連線，它可以是下列狀態其中之一的狀態：

-   **起始：** 當您建立的對等互連從第一個虛擬網路到第二個虛擬網路時，就會顯示。

-   **連接：** 在您已建立對等互連的第二個虛擬網路從第一個虛擬網路之後，就會顯示。 第一個虛擬網路對等互連狀態會從 已起始變更為已連線。 兩個虛擬網路對等必須有已連線的狀態，才能建立虛擬網路對等互連已成功。

-   **中斷連線：** 顯示是否一個虛擬網路中斷連線從另一個虛擬網路。

[狀態的資訊圖]

## <a name="next-steps"></a>後續步驟
[設定虛擬網路對等互連](sdn-configure-vnet-peering.md):在此程序中，您必須使用 Windows PowerShell 來尋找 HNV 提供者邏輯網路建立兩個虛擬網路，每個都有一個子網路。 您也可以設定兩個虛擬網路之間對等互連。

