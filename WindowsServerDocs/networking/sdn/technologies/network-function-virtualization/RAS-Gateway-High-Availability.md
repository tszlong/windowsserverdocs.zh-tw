---
title: RAS 閘道可用性
description: 您可以使用本主題以深入了解可用性設定 RAS Multitenant 閘道的軟體定義網路 (SDN) 在 Windows Server 2016。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 48794ff8312ca00eda25f6d8bdca9929fc47084f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-high-availability"></a>RAS 閘道可用性

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以深入了解可用性設定 RAS Multitenant 閘道的軟體定義網路 (SDN)。  
  
本主題包含下列各節。  
  
-   [RAS 閘道概觀](#bkmk_overview)  
  
-   [閘道集區概觀](#bkmk_pools)  
  
-   [RAS 閘道部署概觀](#bkmk_deployment)  
  
-   [Network Controller 的 RAS 閘道整合](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>RAS 閘道概觀  
如果您的組織雲端服務提供者 (CSP) 或使用多個 tenants 企業版，您可以在 multitenant 模式提供的網路流量路由 virtual 和實體網路，包括網際網路的部署 RAS 閘道。  
  
您可以在路由傳送至承租人 virtual 網路和資源承租人客戶網路流量 edge 閘道 multitenant 模式部署 RAS 閘道。  
  
當您部署 RAS 閘道 Vm 提供可用性和容錯移轉多次時，您的部署閘道集區。 在 Windows Server 2012 R2 所有閘道 Vm 都構成進行閘道部署的邏輯分離有點難的單一集區。  Windows Server 2012 R2 閘道提供閘道 Vm，導致 VPN 連接到網站 (S2S) 提供容量的置中使用 1:1 冗餘部署。  
  
這個問題會提供多個閘道集區的可不同類型的邏輯分隔的 Windows Server 2016 中解析。 新的 [M + N 冗餘模式可讓更有效率容錯移轉設定。  
  
如需概觀 RAS 閘道，請查看[RAS 閘道](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。  
  
## <a name="bkmk_pools"></a>閘道集區概觀  
在 Windows Server 2016，您可以部署閘道一或多個集區中。  
  
下圖顯示閘道集區提供路由 virtual 網路間流量之不同類型。  
  
![RAS 閘道集區](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
每個集區有下列屬性。  
  
-   每個集區是 M + N 備援。 這表示已 ' 的作用中閘道 Vm 備份的待命閘道 Vm「n」數目。 N（待命閘道）值都小於 M（作用中閘道）。  
  
-   集區可以執行任何個人閘道函式-2 (IKEv2) S2S、3 層 (L3)，以及一般路由封裝 (GRE)-或集區網際網路金鑰交換版本可以執行所有的這些功能。  
  
-   您可以將單一公用 IP 位址指派給所有集區或子集集區中。 讓大幅減少公用 IP 位址，您必須使用，因為它是可以讓所有 tenants 連接到上一個 IP 位址雲端。 區段下面可用性和負載平衡描述運作方式。  
  
-   您可以輕鬆地縮放閘道集區向上或向下新增或移除閘道 Vm 集區中。 移除或額外的閘道不會不會中斷集區所提供的服務。 您也可以新增與移除閘道整個集區。  
  
-   連接的單一房客可以在多個集區與集區中的多個閘道終止。 不過，如果房客已連接終止在**所有**輸入閘道集區中，它不希望其他**所有**類型或個人類型閘道集區。  
  
閘道集區也提供其他案例，以便處於：  
  
-   單一-承租人集區的您可以建立集區一承租人來使用。  
  
-   如果您銷售透過協力廠商（轉售商）頻道的雲端服務，您可以建立集區的不同設定為每個轉售商。  
  
-   多個集區，可提供相同的閘道功能，但不同的容量。 例如，您可以建立支援高輸送量和低 IKEv2 S2S 輸送量閘道集區。  
  
## <a name="bkmk_deployment"></a>RAS 閘道部署概觀  
下圖示範 RAS 閘道一般雲端服務提供者 (CSP) 部署。  
  
![RAS 閘道部署概觀](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
使用這類部署，部署後面軟體負載平衡器 (SLB)，如此可讓指派整個部署單一公用 IP 位址 CSP 閘道集區。 在多個閘道集區-和也集區中的多個閘道，可以終止房客的多個閘道器連接。 這透過 IKEv2 S2S 連接上述圖表中所示，但也是一樣適用於其他閘道功能，例如 L3 和 GRE 閘道。  
  
圖示，在威靈頓 BGP 裝置是 RAS Multitenant 閘道 BGP 使用。 Multitenant BGP 用於動態路由。 適用於房客路由集中-單點，稱為「之前的路徑反映 (RR)、處理所有承租人網站 BGP 對都等。 本身 RR 分散在所有閘道集區中。 這會導致位置的房客（資料路徑）連接終止在多個閘道，但 RR 承租人（BGP 等點-控制路徑）的閘道只有一個的設定。  
  
BGP 路由器分開描述此打造路由概念圖。 閘道 BGP 還提供路由傳送，可讓做為傳輸點之間的網站有兩個承租人路由雲端。 這些 BGP 功能適用於所有閘道功能。  
  
## <a name="bkmk_integration"></a>Network Controller 的 RAS 閘道整合  
在 Windows Server 2016 Network Controller 完全整合 RAS 閘道。 當部署 RAS 閘道和 Network Controller Network Controller 執行下列功能。  
  
-   閘道集區的部署  
  
-   在每個閘道承租人連接的設定  
  
-   切換網路流量流向一個待命閘道閘道失敗事件  
  
下列章節提供 RAS 閘道和 Network Controller 的詳細的資訊。  
  
-   [提供與負載平衡的閘道器連接（IKEv2、L3，以及 GRE）](#bkmk_provisioning)  
  
-   [適用於 IKEv2 S2S 可用性](#bkmk_ike)  
  
-   [適用於 GRE 可用性](#bkmk_gre)  
  
-   [轉送閘道 L3 的高可用性](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>提供與負載平衡的閘道器連接（IKEv2、L3，以及 GRE）  
當房客要求閘道器連接時，以 Network Controller 傳送要求。 設定 network Controller 的所有閘道集區，包括每個集區與每個閘道容量每個集區中的相關資訊。 Network Controller 選取正確的集區和閘道器連接。 這個選項根據頻寬需求的連接。 Network Controller 以挑選連接有效率集區使用「最適合」的演算法。 如果這是第一個承租人連接連接的 BGP 等點也指定這一次。  
  
選取 [網路控制器 RAS 閘道器連接之後，Network Controller provisions 上閘道器連接必要的設定。 如果連接 IKEv2 S2S 連接，Network Controller 也 provisions 網路位址轉譯 (NAT) 規則 SLB 集區。這項 SLB 集區規則 NAT 指示承租人從指定閘道器連接要求。 唯一必須來源 IP 來區分 tenants。  
  
> [!NOTE]  
> L3 和 GRE 連接略過 SLB，和直接與指定的 RAS 閘道器連接。  這些連接需要的遠端端點路由器（或其他協力廠商的裝置）必須設定正確的 RAS 閘道器連接。  
  
如果 BGP 路由是否可供連接，然後 BGP 外面 RAS 閘道-車載機起始且路徑會先和雲端交換閘道。 由 BGP 學習（或的靜態設定的路徑如果 BGP 不是）路由傳送至 Network Controller。 Network Controller 然後 plumbs 下 HYPER-V 主機在其安裝承租人 Vm 的路徑。 此時，承租人流量可傳送到正確先網站。 Network Controller 也建立指定閘道位置的相關的 HYPER-V 網路模擬原則和 plumbs 它們下 HYPER-V 主機。  
  
### <a name="bkmk_ike"></a>適用於 IKEv2 S2S 可用性  
連接和 BGP 外面的不同 tenants 所組成 RAS 閘道集區中。 每個集區已 ' 使用閘道和「n」待命閘道。  
  
Network Controller 會以下列方式處理閘道失敗。  
  
-   Network Controller 持續 ping 閘道中的所有集區與可偵測失敗閘道或失敗。 Network Controller 可以偵測下列幾種 RAS 閘道失敗。  
  
    -   RAS 閘道 VM 失敗  
  
    -   HYPER-V 主機時，RAS 閘道執行的失敗  
  
    -   RAS 閘道服務失敗  
  
    Network Controller 儲存所有部署使用閘道的設定。 設定所組成連接設定和路由設定。  
  
-   閘道失敗時，它會影響承租人連接閘道，以及其他閘道位於，但其 RR 位於失敗閘道承租人連接。 向下時間的第二個連接小於前者。 當 Network Controller 偵測閘道失敗時，它會執行下列工作。  
  
    -   移除主機運算影響連接的路徑。  
  
    -   移除這些主機 HYPER-V 網路模擬原則。  
  
    -   選取一個待命閘道、將它轉換成使用閘道，並設定閘道。  
  
    -   變更 NAT 對應至點連接到新閘道 SLB 集區。  
  
-   同時，設定會出現在新作用中閘道，IKEv2 S2S 連接和 BGP 外面是重新建立。 可以雲端閘道或先閘道初始連接和 BGP 對等。 閘道重新整理他們路徑，然後將它們傳送給 Network Controller。 Network Controller 學習閘道探索的新路由之後，Network Controller 傳送路徑和相關的 HYPER-V 網路模擬原則至 HYPER-V 主機的錯誤影響 tenants Vm 所在的位置。 這項活動 Network Controller 是類似的新的安裝程式連接情況下，僅它發生更大。  
  
### <a name="bkmk_gre"></a>適用於 GRE 可用性  
Network Controller-包括偵測、複製連接和待命閘道路由設定，容錯 BGP/靜態路由影響的連接（包括收回及路徑的在重新配管計算主機和 BGP 重新對等），和重新運算主機 HYPER-V 網路模擬原則設定的移轉-RAS 閘道容錯移轉回應的程序也適用於 GRE 閘道和連接。 重新建立的 GRE 連接交貨以不同的方式，但是，且 GRE 的可用性方案部分的額外需求。  
  
![適用於 GRE 可用性](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
閘道部署的時間，請在每個 RAS 閘道 VM 指派動態 IP 位址 (DIP)。 此外，每個閘道 VM 也已指派 GRE 可用性 virtual IP 位址 (VIP)。 Vip 已指派給閘道可接受 GRE 連接集區中，而不非 GRE 集區。 指派 Vip 的通知架 (TOR) 參數使用 BGP，再到雲端實體網路進一步通知 Vip 頂端。 這樣可閘道可從遠端路由器或第三方裝置所在的另一端 GRE 連接。 BGP 對等是不同的承租人路徑交換對等承租人層級 BGP。  
  
GRE 連接提供的時間，在 Network Controller 選取閘道、設定 GRE 端點上選取閘道，並傳回 VIP 指派閘道位址。 這個 VIP 然後設定為 GRE 通道位址遠端路由器上的目的地。  
  
閘道失敗時，Network Controller 複製待命閘道 VIP 位址失敗的閘道及其他設定資料。 待命閘道使用中時，它通知其 TOR 切換至 VIP，並進一步進入實體網路。 遠端路由器繼續相同 VIP 連接 GRE 通道並路由基礎結構確保的新作用中閘道路由封包。  
  
### <a name="bkmk_l3"></a>轉送閘道 L3 的高可用性  
HYPER-V 網路模擬 L3 轉接閘道是橋樑 datacenter 中的實體基礎結構和模擬的 HYPER-V 網路模擬雲端基礎結構。 Multitenant L3 轉接閘道在每個承租人使用它自己的 VLAN 標記邏輯網路連接承租人的實體網路的問題。  
  
當新的承租人建立新的 L3 閘道時，網路控制器閘道服務管理員選取可用閘道 VM，並設定新的承租人介面與可用性（從承租人的 VLAN 標記邏輯網路）客戶地址 (CA) 空間 IP 位址。 使用遠端（實體網路），閘道上對等 IP 位址的 IP 位址，以及為下一步躍瑞曲之戰承租人的 HYPER-V 網路模擬網路。  
  
然而 IPsec 或 GRE 網路連接 TOR 開關切換至將會不了解承租人的 VLAN 標記的網路動態。 路由承租人的 VLAN 標記網路需要上 TOR 開關切換至所有中繼參數和實體基礎結構和確保端點連接閘道路由器設定。  以下是範例 CSP Virtual 網路設定為下列圖所示。  
  
|網路|子網路|VLAN ID|預設閘道|  
|-----------|----------|-----------|-------------------|  
|Contoso L3 邏輯網路|10.127.134.0/24|1001|10.127.134.1|  
|Woodgrove L3 邏輯網路|10.127.134.0/24|1002|10.127.134.1|  
  
以下是範例承租人閘道設定為下列圖所示。  
  
|承租人名稱|L3 閘道 IP 位址|VLAN ID|等 IP 位址|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
以下是這些設定 CSP 資料中心中的圖示。  
  
![轉送閘道 L3 的高可用性](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
閘道失敗的原因，失敗偵測和閘道容錯移轉的部分 L3 轉寄閘道程序很類似 IKEv2 和 GRE RAS 閘道的處理程序。 不同的外部 IP 位址的處理方式。  
  
健康閘道 VM 狀態時，選取其中一個待命閘道從集區 Network Controller，並重新 provisions 網路連接和路由待命閘道。 移動連接，L3 轉寄閘道的高度時提供 CA 空間 IP 位址也移動到新閘道 VM 承租人 CA 空間 BGP IP 位址。  
  
因為 L3 外面 IP 位址移到新閘道 VM 容錯移轉期間，是一次連接到此 IP 位址，以及接下來，瑞曲之戰 HYPER-V 網路模擬工作負載遠端實體的基礎結構。 適用於 BGP 動態路由，BGP IP 位址移動到新閘道 VM CA 空間遠端 BGP 路由器可以重新對等，並將該名了解所有 HYPER-V 網路模擬路徑再試一次。  
  
> [!NOTE]  
> 為了 VLAN 標記邏輯網路使用承租人通訊，您必須分開設定 TOR 切換和中繼路由器的所有。 此外，L3 容錯移轉會用這種方式設定架限制。 因此，必須仔細設定 L3 閘道集區且手動設定必須完成另行購買。  
  


