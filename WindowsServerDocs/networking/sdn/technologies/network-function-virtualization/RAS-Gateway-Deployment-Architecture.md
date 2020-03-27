---
title: RAS 閘道部署架構
description: 您可以使用本主題來瞭解 Windows Server 2016 中 RAS 閘道的雲端服務提供者（CSP）部署，包括 RAS 閘道集區、路由反映程式，以及為個別租使用者部署多個閘道。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 91d8081261d3cbc5e2da61cc2b5a9737e76a0dc7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309809"
---
# <a name="ras-gateway-deployment-architecture"></a>RAS 閘道部署架構

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解 RAS 閘道的雲端服務提供者（CSP）部署，包括 RAS 閘道集區、路由反映程式，以及為個別租使用者部署多個閘道。  
  
下列各節提供一些 RAS 閘道新功能的簡要概述，讓您可以瞭解如何在閘道部署的設計中使用這些功能。  
  
此外，也會提供範例部署，包括有關新增租使用者、路由同步處理和資料平面路由、閘道和路由反映程式容錯移轉等程式的資訊。  
  
本主題包含下列各節。  
  
-   [使用 RAS 閘道的新功能來設計您的部署](#bkmk_new)  
  
-   [範例部署](#bkmk_example)  
  
-   [新增租使用者和客戶位址（CA）空間 EBGP 對等互連](#bkmk_tenant)  
  
-   [路由同步處理和資料平面路由](#bkmk_route)  
  
-   [網路控制站如何回應 RAS 閘道和路由反映器容錯移轉](#bkmk_failover)  
  
-   [使用新的 RAS 閘道功能的優點](#bkmk_advantages)  
  
## <a name="using-ras-gateway-new-features-to-design-your--deployment"></a><a name="bkmk_new"></a>使用 RAS 閘道的新功能來設計您的部署  
RAS 閘道包含多項新功能，可變更並改善您在資料中心內部署閘道基礎結構的方式。  
  
### <a name="bgp-route-reflector"></a>BGP 路由反映  
邊界閘道協定（BGP）路由反映程式功能現在隨附于 RAS 閘道，並提供 BGP 完整網狀拓朴的替代方案，通常是在路由器之間路由同步處理的必要項。 透過完整網狀同步處理，所有 BGP 路由器都必須與路由拓撲中的所有其他路由器連接。 不過，當您使用路由反射器時，路由反映是唯一與所有其他路由器（稱為 BGP 路由反映程式用戶端）連接的路由器，因此可簡化路由同步處理並減少網路流量。 路由反映會學習所有路由、計算最佳路由，並將最佳路由轉散發至其 BGP 用戶端。  
  
如需詳細資訊，請參閱[RAS 閘道的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="gateway-pools"></a><a name="bkmk_pools"></a>閘道集區  
在 Windows Server 2016 中，您可以建立許多不同類型的閘道集區。 閘道集區包含許多 RAS 閘道實例，並在實體和虛擬網路之間路由傳送網路流量。  
  
如需詳細資訊，請參閱[Ras 閘道的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[Ras 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>閘道集區的擴充性  
您可以藉由在集區中新增或移除閘道 Vm，輕鬆地相應增加或減少閘道集區。 移除或新增閘道並不會中斷集區所提供的服務。 您也可以新增和移除整個閘道集區。  
  
如需詳細資訊，請參閱[Ras 閘道的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[Ras 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>M + N 閘道集區冗余  
每個閘道集區都是 M + N 多餘的。 這表示會由 ' N ' 個待命閘道 Vm 數目來備份作用中閘道 Vm 的數目。 M + N 冗余可讓您更有彈性地決定部署 RAS 閘道時所需的可靠性層級。  
  
如需詳細資訊，請參閱[Ras 閘道的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[Ras 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="example-deployment"></a><a name="bkmk_example"></a>範例部署  
下圖提供一個範例，其中包含在兩個租使用者（Contoso 和 Woodgrove）和 Fabrikam CSP 資料中心之間設定的站對站 VPN 連線的 eBGP 對等互連。  
  
![透過站對站 VPN 的 eBGP 對等互連](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
在此範例中，Contoso 需要額外的閘道頻寬，因而導致閘道基礎結構設計決策終止 GW3 上的 Contoso 洛杉磯網站，而不是 GW2。 因此，來自不同網站的 Contoso VPN 連線會在兩個不同閘道上的 CSP 資料中心終止。  
  
這兩個閘道（GW2 和 GW3）都是網路控制卡在 CSP 將 Contoso 和 Woodgrove 租使用者新增至其基礎結構時所設定的第一個 RAS 閘道。 因此，這兩個閘道會設定為這些對應客戶（或租使用者）的路由反映程式。 GW2 是 Contoso 路由反射器，而 GW3 是 Woodgrove Route 反映程式-除了是與 Contoso 洛杉磯總部網站的 VPN 連線 CSP 的 RAS 閘道終止點。  
  
> [!NOTE]  
> 一個 RAS 閘道可以根據每個租使用者的頻寬需求，將虛擬和實體網路流量路由傳送到最多100個不同的租使用者。  
  
作為路由反映程式，GW2 會將 Contoso CA Space 路由傳送至網路控制站，而 GW3 會將 Woodgrove CA Space 路由傳送至網路控制站。  
  
網路控制站會將 Hyper-v 網路虛擬化原則推送至 Contoso 和 Woodgrove 虛擬網路，以及 ras 原則至 RAS 閘道，並將原則負載平衡至設定為軟體負載平衡的 Multiplexers (（Mux）資源.  
  
## <a name="adding-new-tenants-and-customer-address-ca-space-ebgp-peering"></a><a name="bkmk_tenant"></a>新增租使用者和客戶位址（CA）空間 eBGP 對等互連  
當您簽署新客戶，並將客戶新增為資料中心內的新租使用者時，您可以使用下列程式，其中大部分是由網路控制站和 RAS 閘道 eBGP 路由器自動執行。  
  
1.  根據您的租使用者需求，布建新的虛擬網路和工作負載。  
  
2.  如有需要，請設定遠端租使用者企業網站與其位於您資料中心之虛擬網路之間的遠端連線。 當您為租使用者部署站對站 VPN 連線時，網路控制卡會自動從可用的閘道集區中選取可用的 RAS 閘道 VM，並設定連線。  
  
3.  為新的租使用者設定 RAS 閘道 VM 時，網路控制卡也會將 RAS 閘道設定為 BGP 路由器，並將其指定為租使用者的路由反映程式。 即使 RAS 閘道做為閘道，或做為閘道和路由反映程式，也適用于其他租使用者。  
  
4.  根據 CA 空間路由是否設定為使用靜態設定的網路或動態 BGP 路由，網路控制站會在 RAS 閘道 VM 和路由反射器上設定對應的靜態路由、BGP 鄰近專案或兩者。  
  
    > [!NOTE]  
    > -   當網路控制站設定了租使用者的 RAS 閘道和路由反映程式後，每當相同的租使用者需要新的網站間 VPN 連線時，網路控制卡會檢查此 RAS 閘道 VM 上的可用容量。 如果原始閘道可以服務所需的容量，則也會在相同的 RAS 閘道 VM 上設定新的網路連線。 如果 RAS 閘道 VM 無法處理額外的容量，網路控制卡會選取新的可用 RAS 閘道 VM，並在其上設定新的連接。 與租使用者相關聯的新 RAS 閘道 VM 會成為原始租使用者 RAS 閘道路由反射器的路由反映器用戶端。  
    > -   因為 RAS 閘道集區位於軟體負載平衡器（SLBs）後方，所以租使用者的站對站 VPN 位址會分別使用單一公用 IP 位址（稱為虛擬 IP 位址（VIP）），其會由 SLBs 轉譯為資料中心內部 IP 位址，稱為動態 IP 位址（DIP），適用于路由傳送企業租使用者流量的 RAS 閘道。 此由 SLB 進行的公用到私人 IP 位址對應可確保在企業網站與 CSP RAS 閘道和路由反映程式之間，已正確建立站對站 VPN 通道。  
    >   
    >     如需 SLB、Vip 和 Dip 的詳細資訊，請參閱[SDN 的&#40;軟體&#41;負載平衡 SLB](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
5.  為新的租使用者建立企業網站與 CSP 資料中心 RAS 閘道之間的站對站 VPN 通道之後，與通道相關聯的靜態路由會自動布建在通道的企業和 CSP 端.  
  
6.  使用 CA 空間 BGP 路由，也會建立企業網站與 CSP RAS 閘道路由反射器之間的 eBGP 對等互連。  
  
## <a name="route-synchronization-and-data-plane-routing"></a><a name="bkmk_route"></a>路由同步處理和資料平面路由  
在企業網站與 CSP RAS 閘道路由反映程式之間建立 eBGP 對等互連之後，路由反映會使用動態 BGP 路由來學習所有企業路由。 路由反映程式會同步處理所有路由反射器用戶端之間的這些路由，讓它們全都以相同的路由集合進行設定。  
  
路由反映程式也會使用路由同步處理，將這些合併路由更新到網路控制卡。 接著，網路控制站會將路由轉譯為 Hyper-v 網路虛擬化原則，並設定網狀架構網路以確保已布建端對端資料路徑路由。 此程式可讓租使用者虛擬網路從租使用者企業網站進行存取。  
  
針對資料平面路由，到達 RAS 閘道 Vm 的封包會直接路由傳送至租使用者的虛擬網路，因為所有參與的 RAS 閘道 Vm 現在都可使用所需的路由。  
  
同樣地，使用 Hyper-v 網路虛擬化原則時，租使用者虛擬網路會將封包直接路由傳送至 RAS 閘道 Vm （而不需要知道路由反映），然後透過站對站 VPN 通道進行企業網站.  
  
另外。 將來自租使用者虛擬網路的流量傳回遠端租使用者企業網站會略過 SLBs，這是一種稱為「直接伺服器回傳」（DSR）的處理常式。  
  
## <a name="how-network-controller-responds-to-ras-gateway-and-route-reflector-failover"></a><a name="bkmk_failover"></a>網路控制站如何回應 RAS 閘道和路由反映器容錯移轉  
以下是兩種可能的容錯移轉案例：一個用於 RAS 閘道路由反映程式用戶端，另一個用於 RAS 閘道路由反映程式-包括網路控制站如何在任一設定中處理 Vm 容錯移轉的相關資訊。  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>RAS 閘道 BGP 路由反映用戶端的 VM 失敗  
當 RAS 閘道路由反映用戶端失敗時，網路控制卡會採取下列動作。  
  
> [!NOTE]  
> 當 RAS 閘道不是租使用者 BGP 基礎結構的路由反映程式時，它是租使用者 BGP 基礎結構中的路由反映程式用戶端。  
  
-   網路控制卡會選取可用的待命 RAS 閘道 VM，並使用失敗的 RAS 閘道 VM 設定來布建新的 RAS 閘道 VM。  
  
-   網路控制卡會更新對應的 SLB 設定，以確保從租使用者網站到失敗的 RAS 閘道的站對站 VPN 通道，已與新的 RAS 閘道正確建立。  
  
-   網路控制卡會在新的閘道上設定 BGP 路由反映用戶端。  
  
-   網路控制站會將新的 RAS 閘道 BGP 路由反射器用戶端設定為作用中。 RAS 閘道會立即開始與租使用者的路由反映程式進行對等互連，以共用路由資訊並啟用對應企業網站的 eBGP 對等互連。  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>RAS 閘道 BGP 路由反射器的 VM 失敗  
當 RAS 閘道 BGP 路由反射器失敗時，網路控制卡會採取下列動作。  
  
-   網路控制卡會選取可用的待命 RAS 閘道 VM，並使用失敗的 RAS 閘道 VM 設定來布建新的 RAS 閘道 VM。  
  
-   網路控制站會在新的 RAS 閘道 VM 上設定路由反映程式，並為新的 VM 指派失敗的 VM 所使用的相同 IP 位址，藉此提供路由完整性（儘管 VM 失敗）。  
  
-   網路控制卡會更新對應的 SLB 設定，以確保從租使用者網站到失敗的 RAS 閘道的站對站 VPN 通道，已與新的 RAS 閘道正確建立。  
  
-   網路控制站會將新的 RAS 閘道 BGP 路由反映器 VM 設定為作用中。  
  
-   路由反映器會立即變成作用中狀態。 會建立企業的站對站 VPN 通道，而路由反映器會使用 eBGP 對等互連並與企業網站路由器交換路由。  
  
-   選取 BGP 路由之後，RAS 閘道 BGP 路由反映程式會更新資料中心內的租使用者路由反映用戶端，並同步處理網路控制站的路由，讓端對端資料路徑可供租使用者流量使用。  
  
## <a name="advantages-of-using-new-ras-gateway-features"></a><a name="bkmk_advantages"></a>使用新的 RAS 閘道功能的優點  
以下是在設計您的 RAS 閘道部署時，使用這些新的 RAS 閘道功能的幾個優點。  
  
**RAS 閘道擴充性**  
  
由於您可以視需要在 RAS 閘道集區中新增任意數目的 RAS 閘道 Vm，因此您可以輕鬆地調整您的 RAS 閘道部署，以優化效能和容量。 將 Vm 新增至集區時，您可以使用任何種類（IKEv2、L3、GRE）的站對站 VPN 連線來設定這些 RAS 閘道，而不需要停機的情況下，消除容量瓶頸。  
  
**簡化的企業網站閘道管理**  
  
當您的租使用者有多個企業網站時，租使用者可以使用一個遠端站對站 VPN IP 位址和單一遠端鄰居 IP 位址來設定所有網站-您的 CSP 資料中心 RAS 閘道 BGP 路由會反映該租使用者的 VIP。 這可簡化您租使用者的閘道管理。  
  
**快速修復閘道失敗**  
  
若要確保快速的容錯移轉回應，您可以將邊緣路由與控制路由器之間的 BGP Keepalive 參數時間設定為短時間間隔，例如小於或等於10秒。 在這個短暫的保持運作間隔中，如果 RAS 閘道 BGP 邊緣路由器失敗，則會快速偵測到失敗，而網路控制卡會遵循先前各節中所提供的步驟。 這種優點可能會減少個別失敗偵測通訊協定的需求，例如雙向轉送偵測（BFD）通訊協定。  
  
 
  


