---
title: RAS 閘道部署架構
description: 您可以使用本主題以了解雲端服務提供者 (CSP) 在 Windows Server 2016，包括 RAS 閘道集區、 路由反映程式，並為個別租用戶中部署多個閘道的 RAS 閘道部署。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3895e25a2af0437deb9eebe4ad89b110cfc9f2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870269"
---
# <a name="ras-gateway-deployment-architecture"></a>RAS 閘道部署架構

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來深入了解雲端服務提供者 (CSP) 部署 RAS 閘道，包括 RAS 閘道集區、 路由反映程式，並為個別租用戶中部署多個閘道。  
  
下列各節提供 RAS 閘道的新功能的簡短概觀，讓您可以了解如何使用這些功能的閘道部署設計中。  
  
此外，會提供部署範例，包括新增新的租用戶、 路由同步處理和資料平面路由、 閘道和路由反映程式容錯移轉時，與更多的程序的相關資訊。  
  
本主題涵蓋下列各節。  
  
-   [若要設計您的部署使用 RAS 閘道的新功能](#bkmk_new)  
  
-   [部署範例](#bkmk_example)  
  
-   [加入新的租用戶和客戶位址 (CA) 空間 EBGP 對等互連](#bkmk_tenant)  
  
-   [路由的同步處理和資料平面路由](#bkmk_route)  
  
-   [RAS 閘道和路由反映程式容錯移轉網路控制站的回應方式](#bkmk_failover)  
  
-   [使用 RAS 閘道的新功能的優點](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>若要設計您的部署使用 RAS 閘道的新功能  
RAS 閘道包含多項新功能的變更，並改善您部署您的閘道基礎結構在您的資料中心的方式。  
  
### <a name="bgp-route-reflector"></a>BGP 路由反映程式  
邊界閘道通訊協定 (BGP) 路由反映程式功能現在是包含 RAS 閘道，提供 BGP 路由器之間的路由同步處理通常需要的完整網狀拓撲的替代方案。 完整網狀的同步處理，所有的 BGP 路由器必須連接與路由拓樸中的所有其他路由器。 當您使用路由反映程式時，不過，路由反映程式是唯一連接之路由器的所有其他路由器，呼叫 BGP 路由反映程式用戶端，藉此簡化路由同步處理並降低網路流量。 路由反映程式學習所有路由、 計算最佳的路由，並轉散發 BGP 用戶端的最佳路由。  
  
如需詳細資訊，請參閱 < [What's New in RAS 閘道](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="bkmk_pools"></a>閘道集區  
在 Windows Server 2016 中，您可以建立不同類型的多個閘道集區。 閘道集區包含 RAS 閘道的許多執行個體，並將實體和虛擬網路之間的網路流量路由傳送。  
  
如需詳細資訊，請參閱 < [What's New in RAS 閘道](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)並[RAS 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_gps"></a>閘道集區擴充性  
您可以輕鬆地會藉由新增或移除閘道 Vm 集區中調整閘道集區增加或相應減少。 移除或加入閘道不會破壞的集區所提供的服務。 您也可以新增和移除的閘道的整個集區。  
  
如需詳細資訊，請參閱 < [What's New in RAS 閘道](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)並[RAS 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_m"></a>M + N 閘道集區備援  
每個閘道集區是 M + N 備援。 這表示，是 'active 閘道的 Vm 數目會由待命的閘道 Vm 的' n ' 數目。 M + N 備援會將您提供更靈活地決定您需要部署 RAS 閘道時的可靠性層級。  
  
如需詳細資訊，請參閱 < [What's New in RAS 閘道](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)並[RAS 閘道高可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_example"></a>部署範例  
下圖提供範例與 eBGP 對等互連透過兩個租用戶、 Contoso 和 Woodgrove，與 Fabrikam CSP 資料中心之間設定站對站 VPN 連線。  
  
![eBGP 對等互連透過站對站 VPN](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
在此範例中，Contoso 會需要額外的閘道頻寬導致終止 Contoso 洛杉磯站台上而不是 GW2 GW3 閘道基礎結構的設計決策。 因為這個緣故，不同的站台的 Contoso VPN 連線會終止在 CSP 資料中心，在兩個不同的閘道。  
  
這兩個 GW2 和 GW3，這些閘道是第一次的 RAS 閘道，CSP 會加入他們的基礎結構中的 Contoso 和 Woodgrove 的租用戶時，由網路控制站設定。 因為這個緣故，這些兩個閘道會設定為路由反映程式適用於這些對應的客戶 （或租用戶）。 GW2 Contoso 路由反映程式，而 GW3 Woodgrove 路由反映程式-除了與 Contoso Los Angeles 總部站台 VPN 連線的 CSP RAS 閘道終止點。  
  
> [!NOTE]  
> 一個 RAS 閘道可以路由傳送虛擬和實體網路流量對於多達一百不同租用戶，根據每個租用戶的頻寬需求。  
  
為路由反映程式，GW2 傳送 Contoso CA 空間路由到網路控制站，並 GW3 將 Woodgrove CA 空間路由傳送至網路控制站。  
  
網路控制站會將 HYPER-V 網路虛擬化原則推送到 Contoso 和 Woodgrove 的虛擬網路，以及 RAS 閘道和負載平衡原則，以設定為軟體負載平衡 Multiplexers (Mux) RAS 原則集區。  
  
## <a name="bkmk_tenant"></a>加入新的租用戶和客戶位址 (CA) 空間 eBGP 對等互連  
當您登入新的客戶，並將客戶新增為新的租用戶，資料中心內時，您可以使用下列程序，有許多都自動執行網路控制站和 RAS 閘道 eBGP 路由器。  
  
1.  新的虛擬網路和工作負載，根據您的租用戶需求佈建。  
  
2.  如有需要，請在您的資料中心中設定遠端租用戶企業網站與他們的虛擬網路之間的遠端連線。 當您部署租用戶的站對站 VPN 連線時，網路控制站會自動從可用的閘道集區中選取可用的 RAS 閘道 VM，並設定連接。  
  
3.  在新的租用戶設定 RAS 閘道 VM，網路控制站，也會設定為 BGP 路由器的 RAS 閘道，並將它指定為路由反映程式中，租用戶。 這是即使在情況下，則為 true，RAS 閘道可做為閘道，或做為閘道和路由反映程式，其他租用戶。  
  
4.  根據是否 CA 空間路由設定為使用靜態設定的網路或動態的 BGP 路由，網路控制站會設定對應的靜態路由、 BGP 鄰近項目，或同時在 RAS 閘道 VM 和路由反映程式。  
  
    > [!NOTE]  
    > -   網路控制站設定 RAS 閘道和路由反映程式的租用戶之後，每當在相同的租用戶需要新的站對站 VPN 連線，網路控制站會檢查此 RAS 閘道 VM 上的可用容量。 如果原始的閘道可以服務所需的容量，新的網路連線也會在相同的 RAS 閘道 VM 上設定。 RAS 閘道 VM 無法處理額外的容量，如果網路控制卡選取具有新的可用性 RAS 閘道 VM，並在其上設定新的連接。 這個新 RAS 閘道的 VM 相關聯的租用戶會成為原始的租用戶 RAS 閘道路由反映程式的路由反映程式用戶端。  
    > -   租用戶的站對站 VPN 位址，而每個使用單一公用 IP 位址，稱為虛擬 IP 位址 (VIP)，這由 SLBs 轉譯成的資料中心內部 IP 位址，因為 RAS 閘道集區位於軟體負載平衡器 (SLBs) 後方，請呼叫動態 IP 位址 (DIP)，將流量路由傳送企業租用戶的 RAS 閘道。 SLB 這個公開和私密金鑰 IP 位址對應可確保企業網站和 CSP RAS 閘道和路由反映程式之間會正確地建立站對站 VPN 通道。  
    >   
    >     如需有關 SLB Vip，，Dip 的詳細資訊，請參閱[軟體負載平衡&#40;SLB&#41;適用於 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
5.  在企業網站與 RAS 閘道，會建立新的租用戶的 CSP 資料中心之間站對站 VPN 通道之後, 與通道相關聯的靜態路由會自動佈建在企業和 CSP 的通道.  
  
6.  使用 CA 空間 BGP 路由、 企業網站與 CSP RAS 閘道路由反映程式之間的對等互連 eBGP 也建立。  
  
## <a name="bkmk_route"></a>路由的同步處理和資料平面路由  
企業網站與 CSP RAS 閘道路由反映程式之間 eBGP 對等互連建立之後，路由反映程式的所有企業路由來學習使用 BGP 的動態路由。 路由反映程式會同步處理這些所有路由反映程式的用戶端之間的路由，使所有設定使用相同的路由集合。  
  
路由反射程式也會更新這些彙總的路由，使用路由的同步處理，網路控制站。 網路控制站然後轉譯為 HYPER-V 網路虛擬化原則中的路由，並設定網狀架構網路來確保端對端資料路徑路由已佈建。 此程序讓租用戶虛擬網路可存取租用戶企業網站。  
  
資料平面路由，連線到 RAS 閘道 Vm 的封包會直接路由傳送至租用戶的虛擬網路，因為必要的路由都供您使用的所有參與的 RAS 閘道 Vm。  
  
同樣地，使用 HYPER-V 網路虛擬化原則，租用戶虛擬網路路由傳送封包直接到 RAS 閘道 Vm （而不需要了解路由反映程式），然後到企業網站透過站對站 VPN 通道.  
  
以外的地方。 從租用戶虛擬網路的遠端租用戶企業網站的回傳流量會略過 SLBs，稱為 Direct Server Return (DSR) 的程序。  
  
## <a name="bkmk_failover"></a>RAS 閘道和路由反映程式容錯移轉網路控制站的回應方式  
以下是兩個可能的容錯移轉案例-一個用於 RAS 閘道路由反映程式的用戶端-，另一個用於 RAS 閘道路由反映程式包括網路控制站處理方式容錯移轉的 Vm 組態中的相關資訊。  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>RAS 閘道 BGP 路由反映程式用戶端的 VM 失敗  
RAS 閘道路由反映程式用戶端失敗時，網路控制站會採取下列動作。  
  
> [!NOTE]  
> RAS 閘道不是路由反射程式的租用戶 BGP 基礎結構，時，路由反映程式中的用戶端的租用戶 BGP 基礎結構。  
  
-   網路控制卡選取可用的待命 RAS 閘道 VM，並佈建失敗的 RAS 閘道 VM 設定新的 RAS 閘道 VM。  
  
-   網路控制站更新相對應的 SLB 組態，以確保從租用戶站台至失敗的 RAS 閘道的站對站 VPN 通道，正確地建立新的 RAS 閘道。  
  
-   網路控制站會在新的閘道上設定 BGP 路由反映程式用戶端。  
  
-   網路控制站會將新的 RAS 閘道 BGP 路由反映程式用戶端設定為 作用中。 RAS 閘道會立即開始使用租用戶的路由反映程式共用的路由資訊，以及啟用對應的企業網站 eBGP 對等互連的對等互連。  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>RAS 閘道的 BGP 路由反映程式的 VM 失敗  
RAS 閘道 BGP 路由反映程式失敗時，網路控制站會採取下列動作。  
  
-   網路控制卡選取可用的待命 RAS 閘道 VM，並佈建失敗的 RAS 閘道 VM 設定新的 RAS 閘道 VM。  
  
-   網路控制站，路由反映程式在 VM 上設定新 RAS 閘道，並指派新的 VM 相同的 IP 位址所使用的故障的 VM，藉此提供路由完整性，儘管 VM 失敗。  
  
-   網路控制站更新相對應的 SLB 組態，以確保從租用戶站台至失敗的 RAS 閘道的站對站 VPN 通道，正確地建立新的 RAS 閘道。  
  
-   網路控制站設定為使用中的新 RAS 閘道 BGP 路由反映程式的 VM。  
  
-   路由反射程式立即變成作用中。 已建立站對站 VPN 通道，企業，且路由反映程式會使用 eBGP 對等互連，並交換與企業網站路由器的路由。  
  
-   BGP 路由的選取範圍之後, 的 RAS 閘道 BGP 路由反映程式更新租用戶中的資料中心的路由反映程式用戶端，並與網路控制站，讓租用戶流量的端對端資料路徑同步處理的路由。  
  
## <a name="bkmk_advantages"></a>使用 RAS 閘道的新功能的優點  
以下是幾個設計您的 RAS 閘道部署時使用這些新的 RAS 閘道功能的優點。  
  
**RAS 閘道延展性**  
  
您可以加入盡可能 RAS 閘道 Vm，因為您需要 RAS 閘道集區，因此您可以輕鬆地調整您的 RAS 閘道部署，以最佳化效能和容量。 當您將 Vm 新增至集區時，您可以設定這些 RAS 閘道，使用站對站 VPN 連線的任何類型 (IKEv2，L3，GRE)，避免出現容量瓶頸不需要停機。  
  
**簡化的企業站台閘道管理**  
  
當您的租用戶具有多個企業網站時，租用戶可以設定具有一個遠端站對站 VPN IP 位址的所有站台和單一遠端芳鄰的 IP 位址-針對該租用戶的 CSP 資料中心 RAS 閘道的 BGP 路由反映程式 VIP。 這可簡化您的租用戶的閘道管理。  
  
**閘道失敗的快速修復**  
  
若要確保快速的容錯移轉的回應，您可以設定 edge 路由和短時間間隔，例如小於或等於 10 秒控制路由器之間的 BGP Keepalive 參數時間。 這個簡短存留時間間隔，RAS 閘道 BGP 邊緣路由器時，快速偵測到失敗與網路控制器遵循上一節中提供的步驟。 此優點可能會降低針對不同的失敗偵測通訊協定，例如雙向轉送偵測 (BFD) 通訊協定的需求。  
  
 
  


