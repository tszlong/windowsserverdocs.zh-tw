---
title: Azure 虛擬網路整合
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5ff685960c5690e1bdda47742d81ec44a38aeb8b
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181674"
---
# <a name="azure-virtual-network-integration"></a>Azure 虛擬網路整合

>適用于： Windows Server 2016 Essentials

當組織採用雲端運算時，很少會一次移動其所有資源100%，而是採取某種方法，其中有些資源位於雲端，有些則仍然位於內部部署。 這種混合式方法可讓組織輕鬆地將一些運算資源移至雲端，但也可讓他們成長其 IT 基礎結構，而不需要取得新的硬體。

在執行這種混合式方法以進行運算時，需要在兩個位置中的資源彼此通訊的順暢方式。 Azure 虛擬網路是一項 Azure 服務，可讓組織建立點對點（P2P）或站對站（S2S）虛擬私人網路，讓在 Azure 中執行的資源（例如虛擬機器和存放裝置）看起來像是在區域網路上，以進行無縫的應用程式和資源存取。

Azure 虛擬網路的設定可能很複雜。 使用 Windows Server Essentials 2016，您可以透過簡單的 wizard 輕鬆設定您的 Azure 虛擬網路，協助您選擇最適合您網路環境的預設值。 如下列螢幕擷取畫面所示，已將新的 Azure 虛擬網路整合工作新增至 Windows Essentials 儀表板的 [Microsoft Cloud 服務] 區段，以引進 Azure 虛擬網路功能，以及提供起始整合的快速連結。

![螢幕擷取畫面，顯示 [Windows Server Essentials 儀表板] 首頁上的 [開始使用] 索引標籤。 在 [開始使用] 索引標籤上，已選取 [服務] 區段，而儀表板會在 [Microsoft Cloud 服務整合] 下方指出 Azure 虛擬網路目前已停用。](media/azure-virtual-network-1.PNG)

當您在上方的螢幕擷取畫面中按一下 [Azure 虛擬網路的**立即整合**] 連結時，將會出現一個對話方塊，要求您登入您的 Microsoft Azure 帳戶。 如果您沒有 Microsoft Azure 帳戶，您可以選擇在此畫面上註冊一個帳戶，這會將您重新導向至 Azure 帳戶註冊入口網站：

![顯示 [與 Azure 虛擬網路整合] 的 [登入 Microsoft Azure] 頁面的螢幕擷取畫面。](media/azure-virtual-network-2.PNG)

登入 Azure 之後，您將會看到選擇要與 Azure 虛擬網路服務建立關聯之訂用帳戶的選項：

![顯示 [與 Azure 虛擬網路整合] 的 [我的 Microsoft Azure 訂用帳戶] 頁面的螢幕擷取畫面。](media/azure-virtual-network-3.PNG)

一旦您選擇要用於 Azure 虛擬網路的 Azure 訂用帳戶，您將會看到建立新的 Azure 虛擬網路的選項，或者，如果已在此訂用帳戶下設定其中一個，則下拉式方塊會顯示 [可用]。 您也會選擇一個 Azure 虛擬網路將用來識別區域網路中資源的局域網路名稱。 最後，您會選擇要在哪一個 Azure 區域託管 Azure 虛擬網路。 選擇實際最接近您區域網路的位置，通常最適合用來將頻寬速度優化，以便與您在 Azure 服務中裝載的資源進行通訊：

![顯示 [與 Azure 虛擬網路整合] 的 [設定 Azure 虛擬網路] 頁面的螢幕擷取畫面。](media/azure-virtual-network-4.PNG)

整合程式的最後一個步驟是設定將用於 S2S VPN 連接的 VPN 裝置。 由於大多數小型企業在其環境中只有少數伺服器，而且缺少 IT 人員來正確設定 VPN 路由器以連線到 Microsoft Azure，因此預設選項是將 Windows Server Essentials 伺服器設定為您區域網路中的資源將連線的 VPN 伺服器，以便存取 Azure 虛擬網路中的資源。 不過，如果您想要在您的環境中使用另一部伺服器做為 VPN 伺服器，或者您想要使用 VPN 路由器，則可以選取這些選項。

由於路由器類型和型號的變化，Windows Server Essentials 不會嘗試自動設定 VPN 路由器。 選取此整合嚮導中的 VPN 路由器，只會針對 Azure 中所需的適當路由設定，通知裝置類型的 Azure 虛擬網路以進行連線。

完成整合嚮導後，會在適用于 Azure 虛擬網路的 Windows Server Essentials 儀表板中看到新的索引標籤：

![螢幕擷取畫面，其中顯示 [Windows Server Essentials 儀表板] 的 [Azure VNet] 頁面。 [Azure 虛擬網路] 索引標籤已選取，並將狀態顯示為 [正在設定]。](media/azure-virtual-network-5.PNG)

>!請注意，在雲端中完成 Azure 虛擬網路的設定可能需要很長的時間（最多30分鐘）。 在這段期間，設定的狀態將會出現在儀表板的 [Azure 虛擬網路狀態] 頁面中。

Azure 虛擬網路的設定完成後，狀態會變更為 [已連線]，並顯示 Azure 虛擬網路的詳細資料，例如 [傳入/輸出]、[閘道 IP 位址]、[本機 IP 位址] 和 [帳戶詳細資料]：

![螢幕擷取畫面，其中顯示 [Windows Server Essentials 儀表板] 的 [Azure VNet] 頁面。 [Azure 虛擬網路] 索引標籤已選取，並將狀態顯示為 [已連線]，且在 [此狀態資訊] 底下會顯示虛擬網路的詳細資料。](media/azure-virtual-network-6.PNG)

在儀表板右側的 [工作] 窗格中，您可以使用 Azure 虛擬網路來執行的各種工作。

-   **中斷與 AZURE VNET 的**連線設定 Azure 虛擬網路是免費的，但在 Azure 中連線到內部部署和其他 Vnet 的 VPN 閘道需要付費。 從 Azure VNET 中斷連線會停止所有計費。

-   **切換 VPN 裝置**如果您想要從 VPN 伺服器變更為 VPN 路由器，此工作可讓您進行切換，並通知 Azure VNET。

-   **設定 AZURE VNET**這項工作可讓您藉由將 Azure VNET 重新導向至 Azure VNET 的 [Azure 入口網站設定] 頁面，來變更其 advanced configuration 選項。

-   重新整理**狀態**重新整理 [狀態] 頁面，更新 Azure VNET 的線上狀態，包括 in/out 資料。

-   **停用 Azure VNET 整合**中斷 Azure VNET 的連線，並從 Windows Server Essentials 儀表板移除整合。 請注意，這不會刪除 Azure VNET，如果您稍後想要將 Azure VNET 與儀表板重新整合，設定仍然會保留在 Azure 中。

-   **深入瞭解 AZURE VNET** [https://azure.microsoft.com/services/virtual-network/](https://azure.microsoft.com/services/virtual-network/) 。

<a name="see-also"></a>另請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)
