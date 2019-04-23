---
title: Azure 虛擬網路整合
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6193d2e2a2c0b85e13d4e9474264d2581bea88fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879359"
---
#<a name="azure-virtual-network-integration"></a>Azure 虛擬網路整合

>適用於：Windows Server 2016 Essentials

當組織一直到雲端運算，很少會移動所有其資源的 100%，但而採取的方法，其中一些資源都在雲端，而有些則仍然在內部部署。 此混合式方法可方便組織不僅將某些運算資源移至雲端，但也可讓它們擴大其 IT 基礎結構，而不需要取得新硬體。

實作此混合式方法來計算，這兩個位置中的資源可順暢地與彼此進行通訊時需要。 Azure 虛擬網路是 Azure 的服務，可讓組織建立的點對點 (P2P) 或站對站 (S2S) 虛擬私人網路，可如同它們是在 Azure （例如虛擬機器和儲存體） 的查詢執行的資源本機網路上進行無縫式的應用程式和資源存取。

Azure 虛擬網路的設定可能相當複雜。 Windows Server Essentials 2016，您可以輕鬆地設定您的 Azure 虛擬網路透過簡易的精靈，可協助您選擇最適合的預設值，為您的網路環境。 介紹 Azure 虛擬網路，以及提供快速連結到起始整合 Windows Essentials 儀表板的 Microsoft 雲端服務 區段下方的螢幕擷取畫面所示，已加入新的 Azure 虛擬網路整合工作.

![螢幕擷取畫面顯示在 Windows Server Essentials 儀表板首頁上的 [開始] 索引標籤中。 在 [開始] 索引標籤中，已選取 [服務] 區段，並儀表板的指示是在 Microsoft 雲端服務整合而目前已停用 Azure 虛擬網路。](media/azure-virtual-network-1.PNG)

當您按一下 **現在整合**連結 Azure 虛擬網路，上面的螢幕擷取畫面會出現一個對話方塊要求您登入您的 Microsoft Azure 帳戶。 如果您沒有 Microsoft Azure 帳戶，您必須註冊在此畫面，會將您重新導向至 Azure 帳戶註冊入口網站上的其中一個選項：

![螢幕擷取畫面顯示的 [整合與 Azure 虛擬網路精靈] 的 [登入 Microsoft Azure] 頁面中。](media/azure-virtual-network-2.PNG)

一旦登入 Azure，系統會向您選擇的訂用帳戶，他們想要關聯的 Azure 虛擬網路服務：

![螢幕擷取畫面顯示的整合與 Azure 虛擬網路精靈 的 我的 Microsoft Azure 訂用帳戶 頁面中。](media/azure-virtual-network-3.PNG)

一旦您已選擇的選項，建立新的 Azure 虛擬網路中，您會看到您想要用於 Azure 虛擬網路的 Azure 訂用帳戶，或如果其中一個已是否已設定此訂用帳戶，下拉式清單方塊會顯示可用。 您也會選擇區域網路的 Azure 虛擬網路將用來識別您的本機網路中資源的名稱。 最後，您會選擇您想要設定他們的 Azure 虛擬網路，裝載哪些 Azure 區域。 選擇最接近您的區域網路的實際位置是通常最適合最佳化頻寬速度，您可能會裝載在自己的 Azure 服務中的資源與通訊：

![螢幕擷取畫面顯示整合與 Azure 虛擬網路精靈 的 設定註冊 Azure 虛擬網路 頁面中。](media/azure-virtual-network-4.PNG)

整合程序的最後一個步驟是設定將用於 S2S VPN 連線的 VPN 裝置。 因為大多數小型企業在其環境中有只有少數的伺服器，而且缺乏 IT 人員能以適當地設定 連接到 Microsoft Azure 的 VPN 路由器，預設的選取範圍會設定 Windows Server Essentials 伺服器與 VPN 伺服器的資源在您的區域網路會連接到才能存取 Azure 虛擬網路中的資源。 不過，如果您想要使用另一部伺服器在您的環境中為 VPN 伺服器，或您想要使用 VPN 路由器，您可以選取這些選項。

由於路由器型別和模型的變異，Windows Server Essentials 不會嘗試自動設定 VPN 路由器。 在此整合精靈中選取 VPN 路由器時，只會通知適當的路由組態需要在 Azure 中連線的裝置類型的 Azure 虛擬網路。

完成整合精靈，新的索引標籤會顯示在 Azure 虛擬網路的 Windows Server Essentials 儀表板：

![螢幕擷取畫面顯示 Azure VNet 頁面的 Windows Server Essentials 儀表板中。 Azure 虛擬網路 索引標籤選取，並將狀態顯示為設定。](media/azure-virtual-network-5.PNG)

>!請注意 Azure 虛擬網路在雲端中的組態可能需要很長的時間，向上到 30 分鐘內完成。 在此期間，會出現在 [Azure 虛擬網路狀態] 頁面的儀表板中設定的狀態。

Azure 虛擬網路的設定完成後，狀態會變更為 已連接，並顯示您的 Azure 虛擬網路，例如資料輸入/輸出、 閘道 IP 位址、 本機 IP 位址和帳戶詳細資料的詳細資料：

![螢幕擷取畫面顯示 Azure VNet 頁面的 Windows Server Essentials 儀表板中。 Azure 虛擬網路 索引標籤已選取，然後將狀態顯示為已連線，並在此狀態資訊會顯示虛擬網路的詳細資料。](media/azure-virtual-network-6.PNG)

在儀表板右側的 [工作] 窗格是您可能需要與 Azure 虛擬網路的各種工作。

-   **中斷連線從 Azure VNET**設定 Azure 虛擬網路是免費的但沒有連接到內部部署和 Azure 中的其他 Vnet 的 VPN 閘道的費用。 中斷與 Azure VNET 的連線，就會停止所有計費。

-   **透過 VPN 裝置切換**，您想要從 VPN 伺服器變更為 VPN 路由器，此工作可讓您進行切換，並通知 Azure VNET。

-   **設定 Azure VNET**這項工作可讓您變更藉由將其重新導向至 Azure 入口網站設定頁面的 Azure VNET 的 Azure VNET 的進階的組態選項。

-   **重新整理狀態**重新整理 [狀態] 頁面中，更新包括資料輸入/輸出的 Azure VNET 的連線狀態。

-   **停用 Azure VNET 整合**中斷連線 Azure VNET，並移除 Windows Server Essentials 儀表板中的整合。 請注意這不會刪除 Azure VNET，如果您想要稍後重新整合 Azure VNET 與儀表板設定仍會在 Azure 中保留。

-   **深入了解 Azure VNET** [ https://azure.microsoft.com/services/virtual-network/ ](https://azure.microsoft.com/services/virtual-network/)。

<a name="see-also"></a>另請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)
