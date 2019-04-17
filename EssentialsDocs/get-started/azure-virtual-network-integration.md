---
title: "Azure 虛擬網路整合"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 21057359967c73d8d9fc434694b8f203ad5c1f6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
#<a name="azure-virtual-network-integration"></a>Azure 虛擬網路整合

>適用於： Windows Server 2016 Essentials

為組織讓他們雲端運算的方式，很少將它們移動所有他們資源 100%一次，但是而不需要其中一些的資源將會在雲端中，某些則仍在場所方法。 混合如此可讓您輕鬆組織不僅到雲端，將某些運算資源的但是也會讓它們 IT 基礎結構擴大而不必取得新的硬體。

當實作電腦運算這個混合方法，無接縫方式在兩個位置的資源彼此需要。 Azure 虛擬網路是可讓您建立點對點 (P2P) 組織的 Azure 服務或到網站 (S2S) virtual 私人網路，即使它們順暢的應用程式和資源存取的區域網路上的外觀 （例如虛擬電腦與儲存空間） Azure 正在執行的資源。

Azure Virtual 網路的設定可以複雜。 您可以輕鬆地設定 Azure Virtual 透過簡單精靈可協助您選擇最適合用於您的網路環境預設值的網路與 Windows Server Essentials 2016。 下方螢幕擷取畫面所示，新 Azure Virtual 網路整合工作已新增至 Windows 程式集儀表板介紹 Azure Virtual 網路，以及提供快速連結起始 integration 的 Microsoft 雲端服務一節。

![在 Windows Server Essentials 儀表板中的 [首頁] 頁面顯示 [開始] 索引標籤螢幕擷取畫面。 在 [開始] 索引標籤，選取 [服務] 區段，並儀表板表示底下 Microsoft 雲端服務整合 Azure Virtual 網路目前已停用。](media/azure-virtual-network-1.PNG)

當您按下**現在整合**連結 Azure Virtual 網路在螢幕擷取畫面，請將會出現一個對話方塊要求您登入您的 Microsoft Azure 帳號。 如果您不 Microsoft Azure 帳號，您將會有簽署在這個畫面上，這會重新導向您報名 Azure account 入口網站的其中一個選項：

![顯示登入至 Microsoft Azure 頁面整合使用 Azure Virtual 網路精靈的螢幕擷取畫面。](media/azure-virtual-network-2.PNG)

在登入 Azure，您將會看到的選項，選擇想要使用 Azure Virtual 之間的關聯的裝機費網路服務：

![顯示我的 Microsoft Azure 裝機費頁面的拷貝 Azure Virtual 網路精靈的螢幕擷取畫面。](media/azure-virtual-network-3.PNG)

一旦您有選取您想要使用 Azure Virtual 網路的 Azure 裝機費您將會看到選項來建立新的 Azure Virtual 網路，或一個已經已安裝在此裝機費，如果下拉式方塊中會顯示並使用。 您也會選擇 Azure Virtual 網路找出您的區域網路中的資源將會使用的區域網路的名稱。 最後，您都可以選擇您想要 Azure Virtual 網路裝載的 Azure 地區。 選擇最您區域網路接近實際的位置，最好通常最佳化頻寬速度通訊，您可能會在他們 Azure 服務主機的資源：

![顯示設定好 Azure Virtual 網路頁面整合使用 Azure Virtual 網路精靈的螢幕擷取畫面。](media/azure-virtual-network-4.PNG)

整合程序的最後一個步驟是可用於 S2S VPN 連接的 VPN 裝置的設定。 由於最小型企業環境中有只有少數伺服器，缺少正確設定 Microsoft Azure 連接的 VPN 路由器 IT 人員預設選項將會設定為在您的區域網路資源才能存取 Azure Virtual 網路中的資源將會連接 VPN 伺服器的 Windows Server Essentials 伺服器。 不過，如果您想要使用另一部伺服器環境中為 VPN 伺服器，或您想要使用 VPN 路由器，您可以選取的選項。

在路由器類型與機型的書寫方式，因為 Windows Server Essentials 不會自動設定 VPN 路由器。 僅選取 integration 精靈中的 VPN 路由器通知 Azure Virtual 網路的裝置類型的適當路由設定 Azure 需要連接。

完成精靈整合，新的索引標籤便會顯示在 Windows Server Essentials 儀表板 Azure Virtual 網路功能：

![顯示 Azure VNet 頁面的 Windows Server Essentials 儀表板螢幕擷取畫面。 Azure Virtual 網路] 索引標籤已選取，並顯示為設定。](media/azure-virtual-network-5.PNG)

>!請注意的完成 Azure Virtual 網路在雲端中的設定可能需要很長的時間，向上至 30 分鐘的時間。 在這段時間，設定的狀態將會出現在 Azure Virtual 網路狀態頁面上的儀表板。

Azure Virtual 網路的設定會在完成後，狀態將會連線變更，並顯示 Azure Virtual 網路例如日推出的資料、 閘道 IP 位址、 本機 IP 位址和 account 詳細資訊的詳細資料：

![顯示 Azure VNet 頁面的 Windows Server Essentials 儀表板螢幕擷取畫面。 Azure Virtual 網路] 索引標籤已選取 [顯示為已連線，並在此狀態的資訊會顯示 virtual 網路的詳細資訊。](media/azure-virtual-network-6.PNG)

儀表板右側的 [工作] 窗格中的各種不同的工作，您可能需要使用 Azure Virtual 網路。

-   **Azure VNET 從中斷**設定 Azure Virtual 網路是免費的但在場所和其他 VNETs Azure 在連接的 VPN 閘道的費用。 中斷 Azure VNET 停止所有的付款。

-   **切換到 VPN 裝置**，您想要變更的 VPN 伺服器 VPN 路由器，這項工作會讓您將開關切換至和通知 Azure VNET。

-   **設定 Azure VNET**這個工作可讓您變更 Azure VNET 的進階的設定選項，請將其重新導向至 Azure 入口網站的設定頁面的 Azure VNET。

-   **重新整理狀態**更新狀態] 頁面上，更新 Azure VNET 包括資料/輸出連接狀態。

-   **停用 Azure VNET 整合**中斷 Azure VNET 與 Windows Server Essentials 儀表板中移除整合。 請注意這不會 delete Azure VNET，如果您想要稍後再重新整合儀表板 Azure VNET 設定仍會在 Azure 保留。

-   **了解更多有關 Azure VNET**[https://azure.microsoft.com/en-us/services/virtual-network/](https://azure.microsoft.com/en-us/services/virtual-network/)。

<a name="see-also"></a>也了
--------
[開始使用 Windows Server Essentials](get-started.md)
