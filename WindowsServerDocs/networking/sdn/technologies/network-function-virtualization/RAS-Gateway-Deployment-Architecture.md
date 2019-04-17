---
title: RAS 閘道部署架構
description: 您可以使用本主題以深入了解在 Windows Server 2016，包括 RAS 閘道集區之前的路徑反映程式，以及部署多個閘道個人 tenants 的 RAS 閘道部署雲端服務提供者 (CSP)。
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
ms.openlocfilehash: 21b101e10dba3d3b9578d6804b4fd92fbbcd2167
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-deployment-architecture"></a>RAS 閘道部署架構

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以深入了解部署 RAS 閘道，包括 RAS 閘道集區之前的路徑反映程式，以及部署多個閘道個人 tenants 的雲端服務提供者 (CSP)。  
  
下列章節提供一些 RAS 閘道的新功能的簡短的概觀，讓您可以了解如何在閘道部署的設計使用這些功能。  
  
此外，部署範例提供，包括新增新 tenants、路由同步處理資料平面路由、閘道與之前的路徑反映容錯移轉，及更多的程序的相關資訊。  
  
本主題包含下列各節。  
  
-   [使用設計部署 RAS 閘道的新功能](#bkmk_new)  
  
-   [部署範例](#bkmk_example)  
  
-   [新增新的 Tenants 和客戶位址 (CA) 空間 EBGP 對等](#bkmk_tenant)  
  
-   [之前的路徑同步處理和資料平面路由](#bkmk_route)  
  
-   [如何 Network Controller 回應 RAS 閘道和之前的路徑反映錯誤後的移轉](#bkmk_failover)  
  
-   [使用 RAS 閘道的新功能的優點](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>使用設計部署 RAS 閘道的新功能  
RAS 閘道包括多個變更，並改善您部署閘道基礎結構資料中心的方式的新功能。  
  
### <a name="bgp-route-reflector"></a>BGP 路由反映  
邊境閘道通訊協定 (BGP) 路由反映功能現在已隨附 RAS 閘道，並提供 BGP 完整網格拓撲通常需要路由同步處理路由器之間的另一個方法。 完整網格同步處理的所有 BGP 路由器必須與所有其他路由器路由拓撲都連接。 當您使用之前的路徑反映時，不過，路由反映是與其他路由器，稱為 BGP 路由反映戶端，藉以簡化路由同步處理和降低網路流量的所有連接只有路由器。 之前的路徑反映學習所有路徑、 計算最佳路徑，並重新其 BGP 戶端的最佳路由分配。  
  
如需詳細資訊，請查看[新 RAS 閘道在](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)。  
  
### <a name="bkmk_pools"></a>閘道集區  
您可以在 Windows Server 2016 建立不同類型的許多閘道集區。 閘道集區包含許多 RAS 閘道的執行個體與路由實體和 virtual 網路間網路流量。  
  
如需詳細資訊，請查看[RAS 閘道] 中的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[RAS 閘道可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_gps"></a>閘道集區擴充性  
您可以輕鬆地縮放閘道集區向上或向下新增或移除閘道 Vm 集區中。 移除或額外的閘道不會不會中斷集區所提供的服務。 您也可以新增與移除閘道整個集區。  
  
如需詳細資訊，請查看[RAS 閘道] 中的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[RAS 閘道可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
### <a name="bkmk_m"></a>M + N 閘道集區冗餘  
每個閘道集區是 M + N 備援。 這表示已 ' 的作用中閘道 Vm 備份的待命閘道 Vm「n」數目。 M + N 冗餘為您提供更具彈性判斷您需要時部署 RAS 閘道可靠性的層級。  
  
如需詳細資訊，請查看[RAS 閘道] 中的新功能](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)和[RAS 閘道可用性](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)。  
  
## <a name="bkmk_example"></a>部署範例  
下圖範例提供 eBGP 網站-VPN 連接兩個 tenants，以 Contoso Woodgrove，並 Fabrikam CSP datacenter 之間設定上對等。  
  
![eBGP 網站-VPN 上對等](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
在此範例中，以 Contoso 需要其他閘道的頻寬，閘道基礎結構設計判斷来終止 Contoso 洛杉磯網站上 GW3 GW2 而導致。 因為，以 Contoso VPN 來自不同的網站終止 CSP datacenter 這兩個不同的閘道上中。  
  
這兩個 GW2 和 GW3，這些閘道已第一次 RAS 閘道 CSP 加入他們的基礎結構 Contoso 和 Woodgrove tenants 時 Network Controller 的設定。 因此，這些這兩個閘道設定為路由反映程式這些對應針對（或 tenants）。 GW2，以 Contoso 路由反映，就必須 GW3 Woodgrove 路由反映-除了正在 VPN 連接，以 Contoso 洛杉磯總部網站的 CSP RAS 閘道結束點。  
  
> [!NOTE]  
> 一個 RAS 閘道可以傳送 virtual 和實體網路流量的最多個數百不同 tenants，根據的每個承租人頻寬需求。  
  
之前的路徑反映程式，GW2 將 Contoso CA 空間路由傳送至網路控制器，並 GW3 將 Woodgrove CA 空間路由傳送至網路控制器。  
  
Network Controller 推入 HYPER-V 網路模擬原則，以 Contoso 和 Woodgrove virtual 網路，以及為 RAS 閘道及負載平衡原則設定為軟體負載平衡集區 Multiplexers (MUXes) RAS 原則。  
  
## <a name="bkmk_tenant"></a>新增新的 Tenants 和客戶位址 (CA) 空間 eBGP Peering  
當您登入新的客戶並將為新房客客戶新增您的資料中心時，您可以使用下列程序，有許多都由 Network Controller and RAS 閘道 eBGP 路由器會自動執行。  
  
1.  提供新的 virtual 網路和工作負載根據您承租人的需求。  
  
2.  如果需要的話，設定遠端承租人企業網站與他們 virtual 網路之間遠端連接在您的資料中心。 當您的承租人部署至網站 VPN 連接時，Network Controller 自動選取可用 RAS 閘道 VM 提供閘道集區中，並設定連接。  
  
3.  時設定的新承租人 RAS 閘道 VM Network Controller，也 RAS 閘道設定為 BGP 路由器，並將它指定為路由反映程式中的承租人。 這是為 true，即使是在環境 RAS 閘道地方做為閘道，或為閘道和之前的路徑反映，適用於其他 tenants。  
  
4.  根據是否 CA 空間路由靜態設定使用網路或動態 BGP 路由設定，Network Controller 設定對應靜態路徑、BGP 鄰居或兩者上 RAS 閘道 VM 反映之前的路徑。  
  
    > [!NOTE]  
    > -   之後 Network Controller 已設定的 RAS 閘道和之前的路徑反映承租人，只要相同承租人需要新的網站來 VPN 連接 Network Controller 檢查是否有可用的容量此 RAS 閘道 VM 上。 如果原始閘道可以服務所需的容量，相同 RAS 閘道 VM 也被設定新的網路。 如果 RAS 閘道 VM 無法處理其他容量，Network Controller 選取新的可用 RAS 閘道 VM 和上設定的新連接。 相關的承租人這個新 RAS 閘道 VM 變成路由反映 client 的原始承租人 RAS 閘道之前的路徑反映。  
    > -   因為 RAS 閘道集區位於軟體負載平衡器 (SLBs)，tenants 的網站來 VPN 位址每次使用單一公用 IP 位址，稱為「virtual IP 位址 (VIP)，這由 SLBs 被翻譯成稱為動態 IP 位址 (DIP)，適用於企業承租人路由流量 RAS 閘道 datacenter 內部 IP 位址。 透過 SLB 此公開私密金鑰--IP 位址對應可確保之間的企業網站 CSP RAS 閘道和之前的路徑反映程式網站-VPN 通道會建立正確。  
    >   
    >     如需有關 SLB、Vip，以及 DIPs 的詳細資訊，請查看[軟體負載平衡和 #40;SLB 與 #41;適用於 SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)。  
  
5.  網站以 VPN 企業網站與 CSP datacenter RAS 閘道建立新的承租人的通道之後, 通道相關聯的靜態路由自動的企業和 CSP 側邊的通道上提供。  
  
6.  使用 CA 空間 BGP 路由、外面之間的企業網站與 CSP RAS 閘道之前的路徑反映 eBGP 也建立。  
  
## <a name="bkmk_route"></a>之前的路徑同步處理和資料平面路由  
EBGP 外面建立之間企業網站與 CSP RAS 閘道之前的路徑反映之後，在之前的路徑反映程式會學習的所有企業路由使用動態 BGP 路由。 之前的路徑反映同步之間的之前的路徑反映用所有的這些路由使它們的所有設定的路徑與的時間。  
  
之前的路徑反映也會更新這些整合的路徑，使用路由同步，網路控制器。 Network Controller 然後路徑轉譯 HYPER-V 網路模擬原則和設定 Fabric 網路，以確保 End-to-End 資料路徑路由提供。 此程序可承租人 virtual 網路承租人企業的無障礙網站。  
  
資料平面路由，瑞曲之戰 RAS 閘道 Vm 的封包會直接傳送到承租人的 virtual 網路，因為現在參與 RAS 閘道 Vm 中的所有可用的所需的路徑。  
  
同樣地，就地 HYPER-V 網路模擬原則，使用承租人 virtual 網路路由傳送封包直接至 RAS 閘道 Vm（而不需要知道路由反映），然後到企業網站上網站-VPN 通道。  
  
此外。 返回流量承租人 virtual 網路從遠端承租人企業網站略過 SLBs，處理程序稱為「直接伺服器傳回 (DSR)。  
  
## <a name="bkmk_failover"></a>如何 Network Controller 回應 RAS 閘道和之前的路徑反映錯誤後的移轉  
以下是兩個可能容錯移轉案例-一個用於 RAS 閘道之前的路徑反映戶端-，一個用於 RAS 閘道之前的路徑反映程式包括 Network Controller 處理方式容錯移轉 vm 中設定的相關資訊。  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>RAS 閘道 BGP 路由反映 Client 的 VM 失敗  
Network Controller RAS 閘道之前的路徑反映 client 失敗時，必須具備下列動作。  
  
> [!NOTE]  
> 當 RAS 閘道不路由反映房客的 BGP 基礎結構時，是路由反映 client 承租人的 BGP 基礎結構。  
  
-   Network Controller 選取待命 RAS 閘道 VM 可用，並 provisions 新 RAS 閘道 VM 失敗 RAS 閘道 VM 的設定。  
  
-   Network Controller 更新確保網站-VPN 通道從承租人網站失敗 RAS 閘道，正確地建立的新 RAS 閘道對應 SLB 設定。  
  
-   Network Controller 設定 BGP 路由反映 client 新閘道上。  
  
-   Network Controller 設定為使用中的新 RAS 閘道 BGP 路由反映 client。 RAS 閘道立即開始使用承租人的之前的路徑反映分享路由的資訊，以便 eBGP 外面對應企業網站對等。  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>適用於 RAS 閘道 BGP 路由反映 VM 失敗  
Network Controller RAS 閘道 BGP 路由反映失敗時，必須具備下列動作。  
  
-   Network Controller 選取待命 RAS 閘道 VM 可用，並 provisions 新 RAS 閘道 VM 失敗 RAS 閘道 VM 的設定。  
  
-   Network Controller 上新 RAS 閘道 VM 中，設定，路由反映，並將新 VM 指派失敗 VM，藉以 VM 失敗許可之前的路徑完整性使用相同的 IP 位址。  
  
-   Network Controller 更新確保網站-VPN 通道從承租人網站失敗 RAS 閘道，正確地建立的新 RAS 閘道對應 SLB 設定。  
  
-   Network Controller 設定為使用中的新 RAS 閘道 BGP 路由反映 VM。  
  
-   立即在之前的路徑反映程式變成作用中。 建立網站-VPN 通道至企業版，並在之前的路徑反映使用 eBGP 外面和交換企業網站路由器的路徑。  
  
-   之後 BGP 路由選取項目，RAS 閘道 BGP 路由反映更新承租人資料中心、路由反映戶端與網路控制器，請讓 End-to-End 資料路徑承租人流量同步路徑。  
  
## <a name="bkmk_advantages"></a>使用 RAS 閘道的新功能的優點  
以下是幾個優點設計 RAS 閘道部署時，使用這些新 RAS 閘道功能。  
  
**RAS 閘道擴充性**  
  
因為您可以新增多個 RAS 閘道 Vm 當您需要 RAS 閘道集區，您可以輕鬆地縮放效能與容量最佳化 RAS 閘道部署。 當您新增 Vm 集區時，您可以使用的網站 VPN 連接任何保證 (IKEv2，L3，GRE)，無下時間與消除容量瓶頸設定這些 RAS 閘道。  
  
**簡化的企業網站閘道管理**  
  
當您承租人有多個企業網站時，承租人可以在所有網站的一個遠端網站-VPN IP 位址和單一遠端鄰居的 IP 位址-CSP 資料中心 RAS 閘道 BGP 路由反映 VIP 該承租人的設定。 這可簡化您 tenants 閘道管理。  
  
**閘道失敗的預覽版修復功能**  
  
若要確保預覽版容錯移轉回應，您可以設定 BGP Keepalive 參數時間 edge 路徑和控制路由器簡短的時間間隔，例如小於 10 秒之間。 使用這個簡短繼續運作的時間間隔，如果 RAS 閘道 BGP edge 路由器失敗，快速偵測失敗和 Network Controller 遵循上一節中所提供的步驟。 利用這個可能降低失敗的另一個偵測通訊協定，例如雙向轉寄偵測 (BFD) 通訊協定的需求。  
  
 
  


