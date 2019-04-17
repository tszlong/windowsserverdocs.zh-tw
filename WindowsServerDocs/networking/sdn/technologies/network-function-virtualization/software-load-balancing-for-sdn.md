---
title: 軟體負載平衡 SDN (SLB)
description: 若要了解軟體負載平衡軟體定義 Windows Server 2016 中的 [網路，您可以使用此主題。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e08afddde9c7be8d955a0cfdaf44f0fc31b8155
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="software-load-balancing-slb-for-sdn"></a>軟體負載平衡 SDN \(SLB\)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要了解軟體負載平衡軟體定義 Windows Server 2016 中的 [網路，您可以使用此主題。  

雲端服務提供者 (Csp) 與要部署的軟體定義網路 (SDN) 在 Windows Server 2016 中的企業可以使用軟體負載平衡 (SLB) 平均散發承租人和承租人客戶網路流量分配 virtual 網路資源。 Windows Server SLB 可讓伺服器多個主機相同的工作負載，可用性和延展性。
  
Windows Server SLB 包含下列功能。  
  
-   層級 4 (4) 負載平衡服務 '北南' 及 '東西' TCP 日 UDP 傳輸。  
  
-   公開和內部網路流量負載平衡。  
  
-   和您使用 HYPER-V 網路模擬建立 virtual 網路上區域網路 (Vlan)，支援動態 IP 位址 (DIPs)。  
  
-   健康探查支援。  
  
-   準備好要雲端縮放比例，包括擴充功能，並 multiplexers 和主機代理程式擴充功能。  
  
如需詳細資訊，請查看[軟體負載平衡功能](#bkmk_features)本主題中。  
  
> [!NOTE]  
> 不支援 Vlan Network Controller 的 multitenancy，但是您可以使用 Vlan SLB 服務提供者管理工作負載，例如 datacenter 基礎結構密度網頁伺服器。  
  
使用 Windows Server SLB，您可以調整出您負載平衡使用 SLB Vm 您 VM 工作負載您使用的相同 HYPER-V 運算伺服器上的功能。 因此，SLB 支援快速建立和刪除負載平衡端點所需的 CSP 作業。 此外，Windows Server SLB 支援的每個叢集 gb 數以萬計、 提供簡單提供模式，並輕鬆地查看和縮放。  
  
**SLB 的運作方式**  
  
SLB 的運作方式是對應 virtual IP 位址 (Vip) 動態的 IP 位址 (DIPs) 的雲端服務的資料中心中的資源集的一部分。  
  
Vip 的單一提供公用存取集區的負載平衡 Vm 的 IP 位址。 例如，Vip 是 tenants 和承租人針對可以連接到雲端的資料中心承租人資源，會顯示在網際網路的 IP 位址。  
  
DIPs 的成員負載平衡集區 VIP 背後的 Vm 的 IP 位址。 雲端基礎結構承租人資源於指派 dIPs。  
  
Vip 位於中 SLB 多工器 (MUX)。  MUX 包括一或多個虛擬電腦 (Vm)。  Network Controller 提供的每個 VIP，每個 MUX 與每個 MUX 轉使用邊境閘道通訊協定 (BGP) 廣告到路由器上的實體網路 / 32 為每個 VIP 之前的路徑。  BGP 可路由器實體網路，以：  
  
-   了解 VIP 是可在每個 MUX，即使 MUXes 不同子網路中層級 3 網路上。  
  
-   針對每個 VIP 載入分散所有可用的 MUXes 使用路由相等成本多路徑 (ECMP)。  
  
-   自動偵測 MUX 失敗或移除並停止流量傳送到失敗 MUX。  
  
-   從失敗或移除 MUX 載入分散健康 MUXes。  
  
從網際網路公用流量時，SLB MUX 檢查流量，包含 VIP 為目標，並地圖服務，讓它將會抵達個人 DIP 重新寫入傳輸。 輸入網路流量，此交易以之間 MUX 虛擬電腦 (Vm) 和目的地 DIP 所在 HYPER-V 主機分割兩個步驟執行：  
  
-   負載平衡-MUX 使用 VIP 選取 DIP，封裝封包，並流量送給 HYPER-V 主機 DIP 所在的位置。  
  
-   網路位址轉譯 (NAT)-HYPER-V 主機封裝移除封包、 會轉譯到 DIP VIP、 重新連接埠對應及 DIP VM 轉送給封包。  
  
MUX 知道如何對應正確 DIPs Vip 因為負載平衡定義您可以使用 Network Controller 的原則。 本規則包括通訊協定，前端連接埠後, 端連接埠，以及 distribution 演算法 （5、 3 日，或 2 許多組）。  
  
當回應 Vm 承租人並傳送輸出網路流量回網際網路或遠端承租人位置，因為 NAT 都由 HYPER-V 主機，資料傳輸略過 MUX 和直接移至 edge 路由器從 HYPER-V 主機。 此 MUX 略過程序稱為直接伺服器傳回 (DSR)。  
  
然後輸入的網路流量建立初始網路流量之後，完全略過 SLB MUX。  
  
下圖，client 的電腦執行 DNS 查詢公司 Sharepoint 網站-在本案例中名 Contoso 虛構公司的 IP 位址。 下列程序。  
  
-   DNS 伺服器傳回 client VIP 107.105.47.60。  
  
-   Client vip 傳送 HTTP 要求。  
  
-   實體網路上已經有多個可供瑞曲之戰位於任何 MUX VIP 的路徑。  每個路由器過程中使用 ECMP 要求到達 MUX 之前，請選取下一個區段的路徑。  
  
-   收到要求 MUX 檢查設定的原則，並會看到有兩個 DIPs 使用 10.10.10.5 和 10.10.20.5，來處理 vip 107.105.47.60 要求 virtual 網路上  
  
-   MUX 選取 DIP 10.10.10.5 和封裝讓它可以傳送到包含使用主機 DIP 主機實體網路位址，請使用 VXLAN 封包。  
  
-   主機接收封包封裝，並檢查它。  它會移除封裝，並讓目的地現在已而不是 VIP DIP 10.10.10.5 與傳送流量 DIP vm 重新寫入封包。  
  
-   要求現在已達到伺服器發電廠 2 Contoso Sharepoint 網站。 伺服器產生回應，並將其傳送到 client，做為來源使用其本身的 IP 位址。  
  
-   主機攔截傳出封包 virtual 切換，這會記住中的 client，現在的目的地對 VIP 原始要求。  主機重新寫入來源，以 client 看不到 DIP 地址，將 VIP 封包。  
  
-   主機轉送直接給實體網路使用標準路由表轉送到最後接收回應 client 封包預設閘道封包。  
  
![軟體負載平衡程序](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**負載平衡內部 datacenter 流量**  
  
例如之間承租人資源，但在不同的伺服器上執行 virtual 在相同網路的成員，連接的 Vm HYPER-V Virtual 切換執行 NAT 時載入平衡內部 datacenter、 網路流量  
  
使用內部流量負載平衡第一次要求傳送到並 MUX，選取適當的 DIP，傳送的資料傳輸到 DIP 處理。 從那之後，已建立的流量略過 MUX 來到直接從 VM VM。  
  
**健康探查**  
  
SLB 包含健康探查驗證網路基礎結構，包括下列的健康狀態。  
  
-   連接埠 TCP 探查  
  
-   HTTP 探查連接埠和 URL  
  
然而傳統負載平衡器應用裝置位置探查來自應用裝置並在網路上以 DIP，SLB 探查來自的主機 DIP 位置位於，直接從 SLB 主機代理程式前往 DIP，進一步散布各個主機的工作。  
  
## <a name="bkmk_infrastructure"></a>軟體負載平衡基礎結構  
若要部署的 Windows Server SLB，您必須先部署 Windows Server 2016 中的 Network Controller and 一或多個 SLB MUX Vm。  
  
此外，您必須設定 HYPER-V 主機 SDN 式 HYPER-V Virtual 切換，並確定 SLB 主機代理程式正在執行。  服務主機路由器相等成本多重路徑 (ECMP) 路由並邊境閘道通訊協定 (BGP) 必須支援，必須接受 BGP 等要求 SLB MUXes 的設定。  
  
以下是 SLB 基礎結構的概觀。  

![軟體負載平衡基礎結構](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
下列章節提供詳細資訊 SLB 基礎結構的這些項目。  
  
### <a name="scvmm"></a>SCVMM  
使用 System Center 2016，您可以在 Windows Server 2016，包括 SLB 管理員和健康監視器設定 Network Controller。 您也可以使用 System Center 部署 SLB MUXs 並安裝 SLB 主機代理程式正在執行 Windows Server 2016 和 HYPER-V 的電腦上。  
  
如需 System Center 2016 的詳細資訊，請查看[系統中心 2016年](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/)。  
  
> [!NOTE]  
> 如果您不想使用 System Center 2016，您可以使用 Windows PowerShell 或其他管理應用程式安裝和設定 Network Controller and 其他 SLB 基礎結構。 如需詳細資訊，請查看[使用 Windows PowerShell 部署 Network Controller](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="network-controller"></a>Network Controller  
Network Controller 裝載 SLB 經理，並針對 SLB 執行下列動作。  
  
-   處理程序 SLB 來自透過 Northbound API System Center、 Windows PowerShell 或其他網路管理應用程式的命令。  
  
-   計算 distribution HYPER-V 主機和 SLB MUXes 原則。  
  
-   提供 SLB 基礎結構的健康狀態。  
  
### <a name="slb-mux"></a>SLB MUX  
SLB MUX 處理輸入的網路流量和 Vip 對應至 DIPs，然後正確 DIP 轉送給流量。 每個 MUX 也會使用 BGP edge 路由器發行 VIP 路徑。 MUX 失敗時，可讓使用中 MUXes 轉散發載入故障 MUX-基本上負載平衡器提供負載平衡持續運作 BGP 通知 MUXes。  
  
### <a name="hosts-that-are-running-hyper-v"></a>執行 HYPER-V 主機  
您可以使用電腦正在執行 Windows Server 2016 和 HYPER-V SLB。 HYPER-V 主機上的 Vm 執行任何 HYPER-V 支援的作業系統。  
  
### <a name="slb-host-agent"></a>SLB 主機代理程式  
當部署 SLB 時，您必須使用 System Center、 Windows PowerShell 或其他管理應用程式部署 SLB HYPER-V 主機上每個主機代理程式。 Windows Server 2016 的提供 HYPER-V 支援，包括 Nano Server 的所有版本上，您可以安裝 SLB 主機代理程式。  
  
從 Network Controller SLB 的原則更新接聽 SLB 主機代理程式。 此外，主機代理程式的規則的 SLB 到 SDN 式 HYPER-V Virtual 參數本機電腦上設定。  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN 支援 HYPER-V Virtual 開關切換至  
Virtual 切換至相容 SLB，您必須使用 HYPER-V Virtual 切換管理員或 Windows PowerShell 命令來建立切換，以及您必須再讓 Virtual 篩選平台 (VFP) virtual 切換。  
  
關於讓 VFP 上 virtual 參數，查看 Windows PowerShell 命令[取得-VMSystemSwitchExtension](https://technet.microsoft.com/en-us/library/hh848603.aspx)和[讓-VMSwitchExtension](https://technet.microsoft.com/en-us/library/hh848541.aspx?f=255&MSPPError=-2147217396)。  
  
SDN 支援 HYPER-V Virtual 切換為 SLB 執行下列動作。  
  
-   適用於 SLB 處理資料路徑。  
  
-   輸入的網路流量接收 MUX。  
  
-   傳送給使用 DSR 路由器輸出網路流量 MUX 會略過。  
  
-   執行於 HYPER-V 的 Nano Server 執行個體。  
  
### <a name="bgp-enabled-router"></a>BGP 支援路由器  
BGP 路由器 SLB 執行下列動作。  
  
-   若要使用 ECMP MUX 輸入的流量的路徑。  
  
-   輸出網路流量，使用主機提供的路徑。  
  
-   從 SLB MUX Vip 接聽之前的路徑更新。  
  
-   如果失敗繼續運作，請移除 SLB 旋轉 SLB MUXes。  
  
## <a name="bkmk_features"></a>軟體負載平衡功能  
以下是部分功能和 SLB 的功能。  
  
**核心功能**  
  
-   SLB 提供層級 4 負載平衡 '北南' 及 '東西' TCP 日 UDP 流量服務  
  
-   您可以使用 SLB HYPER-V 網路模擬網路  
  
-   您可以使用 SLB VLAN 網路 DIP vm 連接 SDN 支援 HYPER-V Virtual 切換。  
  
-   一個 SLB 處理多個 tenants  
  
-   SLB 和 DIP 支援延展性，而且低延遲退貨路徑，實作，直接伺服器傳回 (DSR)  
  
-   您也會使用切換 Embedded 小組 （設定） 或單一根輸入/輸出模擬 (SR IOV) 時的 SLB 函式  
  
-   SLB 包含網際網路通訊協定第 4 (IPv4) 支援  
  
-   SLB 網站-閘道案例中，提供 NAT 功能，讓利用單一公用 IP 的所有網站-連接  
  
-   您可以安裝 SLB，包括主機代理程式與 Windows Server 2016 上的 MUX，完整、 核心和 Nano 安裝。  
  
**縮放及效能**  
  
-   準備好要雲端縮放比例，包括擴充功能，並 MUXes 和主機代理程式擴充功能。  
  
-   作用中的其中一個管理員 SLB Network Controller 模組可支援 8 MUX 執行個體  
  
**可用性**  
  
-   您可以將 SLB 部署 2 個以上節點主動日主動設定  
  
-   MUXes 可以新增和移除 MUX 集區中，而不影響 SLB 服務。 這會維持 SLB 可用性時   
    修補個人 MUXes。  
  
-   個人 MUX 執行個體有執行 99%的時間  
  
-   管理實體可健康監視資料  
  
**對齊**  
  
-   您可以部署，並使用 SCVMM 設定 SLB  
  
-   SLB 順暢整合的 Microsoft 裝置，例如 RAS Multitenant 閘道、 Datacenter 防火牆和之前的路徑反映提供 multitenant 整合的邊緣。  
  

