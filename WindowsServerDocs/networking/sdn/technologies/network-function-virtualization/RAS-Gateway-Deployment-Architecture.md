---
title: RAS 閘道部署架構
description: 您可以使用本主題來瞭解雲端服務提供者 (CSP) 部署 Windows Server 2016 中的 RAS 閘道，包括 RAS 閘道集區、路由反映程式，以及為個別租使用者部署多個閘道。
manager: grcusanz
ms.topic: how-to
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: abc36a7e26ab25f7f121538dcadf9761b111a6e2
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949364"
---
# <a name="ras-gateway-deployment-architecture"></a>RAS 閘道部署架構

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解「雲端服務提供者」 (CSP) 部署 RAS 閘道，包括 RAS 閘道集區、路由反映程式，以及為個別租使用者部署多個閘道。

下列各節提供一些 RAS 閘道新功能的簡要說明，讓您可以瞭解如何在閘道部署的設計中使用這些功能。

此外，還提供範例部署，包括新增租使用者的程式、路由同步處理和資料平面路由、閘道和路由反映程式容錯移轉等的相關資訊。

本主題包含下列各節。

-   [使用 RAS 閘道的新功能來設計您的部署](#bkmk_new)

-   [部署範例](#bkmk_example)

-   [將新的租使用者和客戶位址 (CA) 空間 EBGP 對等互連](#bkmk_tenant)

-   [路由同步處理和資料平面路由](#bkmk_route)

-   [網路控制站如何回應 RAS 閘道和路由反映器的容錯移轉](#bkmk_failover)

-   [使用新 RAS 閘道功能的優點](#bkmk_advantages)

## <a name="using-ras-gateway-new-features-to-design-your--deployment"></a><a name="bkmk_new"></a>使用 RAS 閘道的新功能來設計您的部署
RAS 閘道包含多項新功能，可變更和改進您在資料中心部署閘道基礎結構的方式。

### <a name="bgp-route-reflector"></a>BGP 路由反映
邊界閘道協定 (BGP) 路由反映程式功能現在隨附于 RAS 閘道，並提供在路由器之間路由同步處理通常需要的 BGP 完整網狀拓撲的替代方法。 透過完整的網狀同步處理，所有 BGP 路由器都必須與路由拓撲中的所有其他路由器連接。 不過，當您使用路由反映時，路由反映程式是唯一與其他所有路由器（稱為 BGP 路由反映用戶端）連線的路由器，因此可簡化路由同步處理並減少網路流量。 路由反映會學習所有路由、計算最佳路由，以及將最佳路由轉散發至其 BGP 用戶端。

如需詳細資訊，請參閱 [RAS 閘道的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。

### <a name="gateway-pools"></a><a name="bkmk_pools"></a>閘道集區
在 Windows Server 2016 中，您可以建立許多不同類型的閘道集區。 閘道集區包含許多 RAS 閘道實例，並在實體和虛擬網路之間路由傳送網路流量。

如需詳細資訊，請參閱 RAS 閘道和[Ras 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)[的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。

### <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>閘道集區擴充性
您可以藉由新增或移除集區中的閘道 Vm，輕鬆地相應增加或減少閘道集區。 移除或新增閘道不會中斷集區所提供的服務。 您也可以新增和移除整個閘道集區。

如需詳細資訊，請參閱 RAS 閘道和[Ras 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)[的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。

### <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>M + N 閘道集區冗余
每個閘道集區都是 M + N 多餘的。 這表示，使用中閘道 vm 的數目是由 ' N ' 個待命閘道 Vm 所備份。 當您部署 RAS 閘道時，M + N 冗余可讓您更有彈性地判斷所需的可靠性層級。

如需詳細資訊，請參閱 RAS 閘道和[Ras 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)[的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。

## <a name="example-deployment"></a><a name="bkmk_example"></a>部署範例
下圖提供透過在兩個租使用者、Contoso 和 Woodgrove 和 Fabrikam CSP 資料中心間設定的站對站 VPN 連線進行 eBGP 對等互連的範例。

![透過站對站 VPN 的 eBGP 對等互連](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)

在此範例中，Contoso 需要額外的閘道頻寬，進而導致閘道基礎結構設計決策終止 GW3 上的 Contoso 洛杉磯網站，而不是 GW2。 因此，來自不同網站的 Contoso VPN 連線在兩個不同閘道的 CSP 資料中心內終止。

當 CSP 將 Contoso 和 Woodgrove 租使用者新增至其基礎結構時，這兩個閘道（GW2 和 GW3）都是網路控制站所設定的第一個 RAS 閘道。 因此，這兩個閘道會設定為這些對應客戶 (或租使用者) 的路由反映程式。 GW2 是 Contoso Route 反映程式，而 GW3 則是 Woodgrove Route 反映程式，除了作為與 Contoso 洛杉磯總部網站的 VPN 連線的 CSP RAS 閘道終止點之外。

> [!NOTE]
> 視每個租使用者的頻寬需求而定，一個 RAS 閘道可以路由傳送最多100個不同租使用者的虛擬和實體網路流量。

作為 Route 反映程式，GW2 會將 Contoso CA 空間路由傳送到網路控制站，而 GW3 則會將 Woodgrove CA 空間路由傳送到網路控制站。

網路控制站會將 Hyper-v 網路虛擬化原則推送至 Contoso 和 Woodgrove 虛擬網路，以及將 RAS 原則推送至 RAS 閘道，並將原則負載平衡至設定為軟體負載平衡集區的 Multiplexers (Mux) 。

## <a name="adding-new-tenants-and-customer-address-ca-space-ebgp-peering"></a><a name="bkmk_tenant"></a>將新的租使用者和客戶位址 (CA) 空間 eBGP 對等互連
當您簽署新客戶並將客戶新增為資料中心內的新租使用者時，您可以使用下列程式，其中大部分都是由網路控制站和 RAS 閘道 eBGP 路由器自動執行。

1.  根據您租使用者的需求布建新的虛擬網路和工作負載。

2.  如有需要，請設定遠端租使用者企業網站與其在您資料中心的虛擬網路之間的遠端連線。 當您部署租使用者的站對站 VPN 連線時，網路控制站會自動從可用的閘道集區中選取可用的 RAS 閘道 VM，並設定連接。

3.  設定新租使用者的 RAS 閘道 VM 時，網路控制站也會將 RAS 閘道設定為 BGP 路由器，並將其指定為租使用者的路由反映程式。 即使在 RAS 閘道作為閘道，或做為閘道和路由反映程式的其他租使用者，也是如此。

4.  根據 CA 空間路由是否設定為使用靜態設定的網路或動態 BGP 路由，網路控制站會在 RAS 閘道 VM 和路由反映程式上設定對應的靜態路由、BGP 鄰近專案或兩者。

    > [!NOTE]
    > -   在網路控制站設定 RAS 閘道和租使用者的路由反映之後，每當相同的租使用者需要新的站對站 VPN 連線時，網路控制站會檢查此 RAS 閘道 VM 上的可用容量。 如果原始閘道可以服務所需的容量，則也會在相同的 RAS 閘道 VM 上設定新的網路連線。 如果 RAS 閘道 VM 無法處理額外的容量，網路控制站會選取新的可用 RAS 閘道 VM，並在其上設定新的連接。 與租使用者相關聯的新 RAS 閘道 VM 會成為原始租使用者 RAS 閘道路由反映程式的路由反映程式用戶端。
    > -   由於 RAS 閘道集區位於軟體負載平衡器 (SLBs) ，因此租使用者的站對站 VPN 位址都會使用單一公用 IP 位址（稱為虛擬 IP 位址） (VIP) ，該 ip 位址會由 SLBs 轉譯為資料中心內部 IP 位址（稱為動態 IP 位址 (DIP) ），以供路由傳送企業租使用者流量的 RAS 閘道。 此公用到私人 IP 位址對應（依 SLB）可確保企業網站與 CSP RAS 閘道和路由反映程式之間的站對站 VPN 通道已正確建立。
    >
    >     如需 SLB、Vip 和 Dip 的詳細資訊，請參閱 [SDN&#41; &#40;slb 的軟體負載平衡](./software-load-balancing-for-sdn.md)。

5.  為新的租使用者建立企業網站與 CSP datacenter RAS 閘道之間的站對站 VPN 通道後，與通道相關聯的靜態路由會自動布建在通道的企業和 CSP 端。

6.  使用 CA 空間 BGP 路由時，也會建立企業網站與 CSP RAS 閘道路由反映之間的 eBGP 對等互連。

## <a name="route-synchronization-and-data-plane-routing"></a><a name="bkmk_route"></a>路由同步處理和資料平面路由
在企業網站與 CSP RAS 閘道路由反映程式之間建立 eBGP 對等互連之後，路由反映程式會使用動態 BGP 路由來學習所有企業路由。 路由反映程式會同步處理所有路由反映程式用戶端之間的這些路由，讓它們全都使用相同的路由集合進行設定。

路由反映程式也會使用路由同步處理將這些合併路由更新至網路控制站。 然後網路控制站會將路由轉譯為 Hyper-v 網路虛擬化原則，並設定網狀架構網路，以確保已布建端對端資料路徑路由。 此程式可讓租使用者虛擬網路從租使用者企業網站存取。

針對資料平面路由，送達 RAS 閘道 Vm 的封包會直接路由傳送至租使用者的虛擬網路，因為所有參與的 RAS 閘道 Vm 現在都可使用必要的路由。

同樣地，使用 Hyper-v 網路虛擬化原則時，租使用者虛擬網路會將封包直接路由傳送到 RAS 閘道 Vm (而不需要知道路由反映) ，然後再透過站對站 VPN 通道來瞭解企業網站。

此外。 將來自租使用者虛擬網路的流量傳回至遠端租使用者企業網站會略過 SLBs，這是一種稱為「直接伺服器回傳」 (DSR) 的處理常式。

## <a name="how-network-controller-responds-to-ras-gateway-and-route-reflector-failover"></a><a name="bkmk_failover"></a>網路控制站如何回應 RAS 閘道和路由反映器的容錯移轉
以下是兩種可能的容錯移轉案例：一個適用于 RAS 閘道路由反映用戶端，另一個用於 RAS 閘道路由反映程式，包括網路控制站如何針對任一設定中的 Vm 處理容錯移轉的相關資訊。

### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>RAS 閘道 BGP 路由反映用戶端的 VM 失敗
當 RAS 閘道路由反映用戶端失敗時，網路控制站會採取下列動作。

> [!NOTE]
> 當 RAS 閘道不是租使用者 BGP 基礎結構的路由反映時，它是租使用者 BGP 基礎結構中的路由反映程式用戶端。

-   網路控制站會選取可用的待命 RAS 閘道 VM，並使用失敗的 RAS 閘道 VM 設定來布建新的 RAS 閘道 VM。

-   網路控制站會更新對應的 SLB 設定，以確保從租使用者網站到失敗 RAS 閘道的站對站 VPN 通道，已與新的 RAS 閘道正確建立。

-   網路控制站會在新的閘道上設定 BGP 路由反映用戶端。

-   網路控制站會將新的 RAS 閘道 BGP 路由反映用戶端設定為作用中。 RAS 閘道會立即開始對等互連與租使用者的路由反映程式，以共用路由資訊，以及啟用對應企業網站的 eBGP 對等互連。

### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>RAS 閘道 BGP 路由反映的 VM 失敗
當 RAS 閘道 BGP 路由反映失敗時，網路控制站會採取下列動作。

-   網路控制站會選取可用的待命 RAS 閘道 VM，並使用失敗的 RAS 閘道 VM 設定來布建新的 RAS 閘道 VM。

-   網路控制站會在新的 RAS 閘道 VM 上設定路由反映，並將新的 VM 指派為失敗的 VM 所使用的相同 IP 位址，藉此提供路由完整性（儘管 VM 失敗）。

-   網路控制站會更新對應的 SLB 設定，以確保從租使用者網站到失敗 RAS 閘道的站對站 VPN 通道，已與新的 RAS 閘道正確建立。

-   網路控制站會將新的 RAS 閘道 BGP 路由反映 VM 設定為作用中。

-   路由反映器會立即變成作用中狀態。 企業的站對站 VPN 通道已建立，而路由反映程式會使用 eBGP 對等互連，並與企業網站路由器交換路由。

-   在 BGP 路由選取之後，RAS 閘道 BGP 路由反映程式會更新資料中心內的租使用者路由反映用戶端，並同步處理與網路控制站的路由，讓端對端資料路徑可供租使用者流量使用。

## <a name="advantages-of-using-new-ras-gateway-features"></a><a name="bkmk_advantages"></a>使用新 RAS 閘道功能的優點
以下是在設計 RAS 閘道部署時使用這些新 RAS 閘道功能的一些優點。

**RAS 閘道的擴充性**

由於您可以在 RAS 閘道集區中新增任意數目的 RAS 閘道 Vm，因此您可以輕鬆地調整您的 RAS 閘道部署，以優化效能和容量。 當您將 Vm 新增至集區時，您可以使用任何類型的站對站 VPN 連線來設定這些 RAS 閘道， (IKEv2、L3、GRE) 、消除容量瓶頸而無停機時間。

**簡化的企業網站閘道管理**

當您的租使用者有多個企業網站時，租使用者可以設定所有具有一個遠端站對站 VPN IP 位址和一個遠端鄰近 IP 位址的網站，也就是該租使用者的 CSP 資料中心 RAS 閘道 BGP 路由反映 VIP。 這可簡化租使用者的閘道管理。

**快速補救閘道失敗**

若要確保快速的容錯移轉回應，您可以將邊緣路由和控制路由器之間的 BGP Keepalive 參數時間設定為短時間間隔，例如小於或等於10秒。 使用這個短暫的保持運作間隔，如果 RAS 閘道 BGP edge 路由器失敗，就會快速偵測到失敗，而網路控制站會遵循先前各節中提供的步驟。 這種優點可能會降低個別失敗偵測通訊協定（例如雙向轉送偵測 (BFD) 通訊協定）的需求。