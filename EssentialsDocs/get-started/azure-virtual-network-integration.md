---
title: Azure 虛擬網路整合
description: 深入瞭解 Azure 虛擬網路整合，可讓您建立點對點 (P2P) 或站對站 (S2S) 虛擬私人網路。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 98805bb9e5b14b82b64eb7188432b0284aa8c905
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696793"
---
# <a name="azure-virtual-network-integration"></a>Azure 虛擬網路整合

>適用于： Windows Server 2016 Essentials

當組織採用雲端運算的方式時，很少會一次將所有資源移至100%，而是採取一種方法，讓某些資源在雲端中，而某些資源仍在內部部署環境。 這種混合式方法可讓組織輕鬆地將某些運算資源移至雲端，但也可讓他們成長 IT 基礎結構，而不需要取得新的硬體。

當您執行這種混合式方法來進行運算時，這兩個位置中的資源都必須有順暢的通訊方式。 Azure 虛擬網路是一項 Azure 服務，可讓組織建立點對點 (P2P) 或站對站 (S2S) 虛擬私人網路，讓在 Azure 中執行的資源 (例如虛擬機器和儲存體) 看起來像是在區域網路上，以進行無縫的應用程式和資源存取。

設定 Azure 虛擬網路可能很複雜。 透過 Windows Server Essentials 2016，您可以輕鬆地透過簡單的 wizard 設定 Azure 虛擬網路，協助您為您的網路環境選擇最適當的預設值。 如下列螢幕擷取畫面所示，已將新的 Azure 虛擬網路整合工作新增至 Windows Essentials 儀表板的 [Microsoft 雲端服務] 區段，以引進 Azure 虛擬網路功能，並提供快速連結來起始整合。

![顯示 [Windows Server Essentials 儀表板] 首頁上 [開始] 索引標籤的螢幕擷取畫面。 在 [開始] 索引標籤上，已選取 [服務] 區段，而 [儀表板] 在 [Microsoft 雲端服務整合] 下指出 Azure 虛擬網路目前已停用。](media/azure-virtual-network-1.PNG)

當您在上面的螢幕擷取畫面中按一下 [Azure 虛擬網路的 **立即整合** ] 連結時，將會出現一個對話方塊，要求您登入 Microsoft Azure 帳戶。 如果您沒有 Microsoft Azure 帳戶，您可以選擇在此畫面上註冊一個帳戶，這會將您重新導向至 Azure 帳戶註冊入口網站：

![顯示 [與 Azure 虛擬網路整合] 的 [登入 Microsoft Azure] 頁面的螢幕擷取畫面。](media/azure-virtual-network-2.PNG)

登入 Azure 後，您將會看到選擇要與 Azure 虛擬網路服務相關聯之訂用帳戶的選項：

![顯示 [與 Azure 虛擬網路整合] 的 [我的 Microsoft Azure 訂用帳戶] 頁面的螢幕擷取畫面。](media/azure-virtual-network-3.PNG)

選擇要用於 Azure 虛擬網路的 Azure 訂用帳戶之後，您將會看到建立新的 Azure 虛擬網路的選項，或者，如果此訂用帳戶中已有設定，則下拉式方塊將會顯示它可供使用。 您也會為 Azure 虛擬網路將用來識別您區域網路中的資源的區域網路選擇名稱。 最後，您將選擇要裝載其 Azure 虛擬網路的 Azure 區域。 選擇實際最接近區域網路的位置，通常最適合用來將頻寬速度與您可能在其 Azure 服務中裝載的資源進行通訊優化：

![顯示 [與 Azure 虛擬網路整合] 的 [設定 Azure 虛擬網路] 頁面的螢幕擷取畫面。](media/azure-virtual-network-4.PNG)

整合程式的最後一個步驟是設定將用於 S2S VPN 連接的 VPN 裝置。 因為大部分的小型企業在其環境中只有少數伺服器，而且缺乏 IT 人員適當地設定 VPN 路由器來連線 Microsoft Azure，所以預設選項是將 Windows Server Essentials 伺服器設定為您區域網路中的資源將連接的 VPN 伺服器，以便存取 Azure 虛擬網路中的資源。 但是，如果您想要在環境中使用另一部伺服器做為 VPN 伺服器，或您想要使用 VPN 路由器，則可以選取這些選項。

由於路由器類型和模型的變化，Windows Server Essentials 不會嘗試自動設定 VPN 路由器。 在此整合嚮導中選取 VPN 路由器，只會通知裝置類型的 Azure 虛擬網路，以在 Azure 中針對連線能力所需的適當路由設定。

完成整合嚮導之後，適用于 Azure 虛擬網路的 Windows Server Essentials 儀表板中會顯示新的索引標籤：

![顯示 Windows Server Essentials 儀表板 [Azure VNet] 頁面的螢幕擷取畫面。 已選取 [Azure 虛擬網路] 索引標籤，並將狀態顯示為 [設定]。](media/azure-virtual-network-5.PNG)

>!注意在雲端中完成 Azure 虛擬網路的設定可能需要很長的時間（最多30分鐘）。 在這段期間內，設定的狀態將會出現在 [儀表板] 的 [Azure 虛擬網路狀態] 頁面中。

完成 Azure 虛擬網路的設定之後，狀態會變更為 [已連線]，並顯示 Azure 虛擬網路的詳細資料，例如資料傳入/傳出、閘道 IP 位址、本機 IP 位址和帳戶詳細資料：

![顯示 Windows Server Essentials 儀表板 [Azure VNet] 頁面的螢幕擷取畫面。 系統會選取 [Azure 虛擬網路] 索引標籤，並將狀態顯示為 [已連線]，而在此狀態資訊下會顯示虛擬網路的詳細資料。](media/azure-virtual-network-6.PNG)

在儀表板右側的 [工作] 窗格中，您可以使用 Azure 虛擬網路來執行各種工作。

-   **中斷與 AZURE VNET 的** 連線設定 Azure 虛擬網路是免費的，但連線至內部部署的 VPN 閘道和 Azure 中的其他 Vnet 都需要付費。 中斷與 Azure VNET 的連線會停止所有帳單。

-   **切換 VPN 裝置** 如果您想要從 VPN 伺服器變更為 VPN 路由器，此工作可讓您進行切換，並通知 Azure VNET。

-   **設定 AZURE VNET** 這項工作可讓您將 Azure VNET 的設定選項重新導向至 Azure VNET 的 Azure 入口網站設定頁面，以變更其 advanced configuration 選項。

-   重新整理 **狀態** 重新整理狀態頁面，並更新 Azure VNET 的線上狀態，包括資料 in/out。

-   **停用 Azure VNET 整合** 中斷 Azure VNET 的連線，並從 Windows Server Essentials 儀表板移除整合。 請注意，如果您想要稍後重新整合 Azure VNET 與儀表板，則不會刪除 Azure VNET，設定仍會保留在 Azure 中。

-   **深入瞭解 AZURE VNET** [https://azure.microsoft.com/services/virtual-network/](https://azure.microsoft.com/services/virtual-network/) 。

<a name="see-also"></a>請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)
