---
title: SDN 的軟體負載平衡 (SLB)
description: 您可以使用本主題來了解軟體負載平衡適用於 Windows Server 2016 中的軟體定義網路功能。
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
ms.openlocfilehash: 26fb4aa21e80618c4c63bd9edbf8731bf886db62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853759"
---
# <a name="software-load-balancing-slb-for-sdn"></a>軟體負載平衡\(SLB\)適用於 SDN

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解軟體負載平衡適用於 Windows Server 2016 中的軟體定義網路功能。  

雲端服務提供者 (Csp) 和要部署軟體定義網路 (SDN) Windows Server 2016 中的企業可以使用軟體負載平衡 (SLB) 將租用戶和租用戶客戶網路流量在虛擬網路資源之間平均地分散。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。
  
Windows Server SLB 可包含下列功能。  
  
-   第 4 (L4) 層負載平衡的服務，' 北-南 '和' 東-西 ' TCP/UDP 流量。  
  
-   公用和內部網路流量負載平衡。  
  
-   您使用 HYPER-V 網路虛擬化建立的虛擬網路和虛擬區域網路 (Vlan) 上，支援動態 IP 位址 (Dip)。  
  
-   健康情況探查支援。  
  
-   雲端規模，包括向外延展功能，可供和 multiplexers 和主機代理程式擴充功能。  
  
如需詳細資訊，請參閱 <<c0> [ 軟體負載平衡功能](#bkmk_features)本主題中。  
  
> [!NOTE]  
> 網路控制卡所不支援 Vlan，多租用戶，不過，您可以使用 SLB 使用虛擬區域網路服務提供者管理的工作負載，例如資料中心基礎結構和高的密度網頁伺服器。  
  
使用 Windows Server SLB，您可以相應放大您的負載平衡功能，使用您用於其他 VM 工作負載在相同 HYPER-V 計算伺服器上的 SLB Vm。 因為這個緣故，SLB 會支援的快速建立和刪除負載平衡端點所需的 CSP 的作業。 此外，Windows Server SLB 支援十幾個 gb，每個叢集，提供簡單的佈建模式，而且容易相應放大和縮小。  
  
**SLB 的運作方式**  
  
SLB 的運作方式是將虛擬 IP 位址 (Vip) 對應到動態 IP 位址 (Dip) 是資料中心內各項資源的雲端服務集的一部分。  
  
Vip 是負載的提供公用存取權的集區平衡 Vm 的單一 IP 位址。 比方說，Vip 會使租用戶和租用戶的客戶可以連線到雲端資料中心內的租用戶資源，在網際網路公開的 IP 位址。  
  
Dip 是成員的背後的 VIP 負載平衡集區 Vm 的 IP 位址。 Dip 指派給租用戶資源的雲端基礎結構內。  
  
Vip 位在 SLB 多工器 (MUX)。  MUX 是由一或多個虛擬機器 (Vm) 所組成。  網路控制站提供使用每個 VIP，每個 MUX 和每個 MUX 依次使用邊界閘道通訊協定 (BGP) 通告每個 VIP 上實體網路為/32 的路由器的路由。  BGP 可讓實體網路路由器：  
  
-   了解 VIP 是用於每個 MUX，即使是在第 3 層網路中的不同子網路上的多工器。  
  
-   負載分散到每個 VIP 使用多重路徑 (ECMP) 路由的所有可用多工。  
  
-   自動偵測 MUX 失敗或移除，並停止將流量傳送到失敗的 MUX。  
  
-   從失敗或已移除 MUX 負載分散到狀況良好的多工器。  
  
從網際網路的公用流量時，SLB MUX 會檢查流量，包含做為目的地的 VIP 對應，以及重寫流量，因此它將會看見個別的 DIP。 輸入網路流量，此交易被利用 MUX 虛擬機器 (Vm) 與目的地 DIP 所在的 HYPER-V 主機之間分割成兩個步驟：  
  
-   負載平衡-MUX 使用的 VIP，以選取 DIP，封裝的封包，並將流量轉送到 DIP 所在的 HYPER-V 主機。  
  
-   網路位址轉譯 (NAT)-HYPER-V 主機移除封裝從封包、 轉譯到 DIP VIP、 重新對應連接埠，並將封包轉送到 DIP 的 VM。  
  
MUX 知道如何將 Vip 對應至正確的 Dip，因為負載平衡原則，您可以使用網路控制卡定義。 這些規則包括通訊協定、 前端連接埠、 後端連接埠，以及分配演算法 (5，3，或 2 的 tuple)。  
  
當租用戶 Vm 回應並傳送輸出網路流量回到網際網路或在遠端租用戶位置，因為 NAT 由 HYPER-V 主機，而流量會略過 MUX，並會直接移至邊緣路由器從 HYPER-V 主機。 這個 MUX 略過處理程序稱為 Direct Server Return (DSR)。  
  
並將傳入的網路流量建立初始的網路流量流程之後，完全略過 SLB MUX。  
  
在下圖中，用戶端電腦會執行公司 SharePoint 站台-在此情況下，名為 Contoso 的虛構公司的 IP 位址的 DNS 查詢。 下列程序就會發生。  
  
-   DNS 伺服器會傳回給用戶端的 VIP 107.105.47.60。  
  
-   用戶端會傳送 HTTP 要求到 VIP。  
  
-   實體網路具有多個路徑可用來連線到位於任何 MUX 的 VIP。  在過程中每個路由器使用 ECMP，挑選下一個區段的路徑，直到要求抵達 MUX。  
  
-   接收要求 MUX 檢查設定的原則，並看到有兩個下降，10.10.10.5 和 10.10.20.5，來處理要求至 VIP 107.105.47.60 虛擬網路上  
  
-   MUX 選取 DIP 10.10.10.5，並使用 VXLAN，因此它可以傳送給包含 DIP 使用主機的主機實體網路位址的封包封裝。  
  
-   主應用程式收到封裝的封包，並檢查它。  它會移除封裝，並讓目的地現在是 DIP 10.10.10.5 而不是 VIP，並將流量傳送到 DIP 的 VM，請重寫封包。  
  
-   要求現在已達到伺服器陣列 2 Contoso SharePoint 網站。 伺服器會產生回應，並將它傳送到用戶端，使用自己的 IP 位址作為來源。  
  
-   主應用程式會攔截傳出的封包中的虛擬交換器，這會記住，用戶端，現在的目的地，原始要求對 VIP。  主應用程式重寫為 VIP，因此，用戶端不會看到的 DIP 位址的封包的來源。  
  
-   主應用程式會將轉送的封包，直接向它轉送至用戶端，最終會在收到回應封包會使用其標準的路由表的實體網路的預設閘道。  
  
![軟體負載平衡程序](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**負載平衡內部的資料中心流量**  
  
何時負載平衡網路流量內部的資料中心，例如之間不同的伺服器上執行，而且是成員的相同虛擬網路的租用戶資源，Vm 會連線到 HYPER-V 虛擬交換器執行 nat。  
  
負載平衡內部流量，第一個要求會傳送至，並處理 MUX，選取適當的 DIP，並將流量路由傳送到 DIP。 從該點之後，已建立的流量會略過 MUX，並直接從 VM 移至 VM。  
  
**健康情況探查**  
  
SLB 會包含要驗證的網路基礎結構，包括下列健全狀況的健全狀況探查。  
  
-   連接埠的 TCP 探查  
  
-   若要連接埠和 URL 的 HTTP 探查  
  
不同於傳統負載平衡器裝置探查起始應用裝置上的位置，並會透過網路傳輸到 DIP，源自其中 DIP 的所在位置，而影片會直接從 SLB 主機代理程式，以進一步散發的 DIP 在主機上的 SLB 探查運作的各個主機。  
  
## <a name="bkmk_infrastructure"></a>軟體負載平衡的基礎結構  
若要部署 Windows Server SLB，您必須先部署 Windows Server 2016 中的網路控制站和一個或多個 SLB MUX Vm。  
  
此外，您必須使用 SDN 啟用 HYPER-V 虛擬交換器設定 HYPER-V 主機，並確定 SLB 主機代理程式正在執行。  服務主機的路由器必須支援相同成本多重路徑 (ECMP) 路由和邊界閘道通訊協定 (BGP)，而且必須設定為接受來自 SLB Mux 的 BGP 對等互連要求。  
  
以下是 SLB 基礎結構的概觀。  

![軟體負載平衡的基礎結構](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
下列各節提供有關這些元素的 SLB 基礎結構的詳細資訊。  
  
### <a name="scvmm"></a>SCVMM  
使用 System Center 2016，您可以包括 SLB 管理員和健全狀況監視的 Windows Server 2016 上設定網路控制站。 您也可以使用 System Center 部署 SLB MUXs，以及安裝在執行 Windows Server 2016 和 HYPER-V 的電腦上的 SLB 主機代理程式。  
  
如需有關 System Center 2016 的詳細資訊，請參閱 < [System Center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/)。  
  
> [!NOTE]  
> 如果您不想要使用 System Center 2016，您可以使用 Windows PowerShell 或其他管理應用程式來安裝和設定網路控制站和其他 SLB 基礎結構。 如需詳細資訊，請參閱 <<c0> [ 部署網路控制站使用 Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)。  
  
### <a name="network-controller"></a>網路控制卡  
網路控制站裝載 SLB 管理員，並執行下列動作的 SLB。  
  
-   處理來自 Northbound API 透過 System Center、 Windows PowerShell 或其他網路管理應用程式的 SLB 命令。  
  
-   計算以散發給 HYPER-V 主機和 SLB Mux 的原則。  
  
-   提供的 SLB 基礎結構的健全狀況狀態。  
  
### <a name="slb-mux"></a>SLB MUX  
SLB MUX 處理輸入的網路流量，並將 Vip 對應到 Dip，然後將流量轉送到正確的 DIP。 每個 MUX 也會使用 BGP 將 VIP 路由發佈至邊緣路由器。 保持運作 BGP 通知 MUX 失敗時，可讓作用中的多工器，以便分散故障 MUX-本質上提供的負載平衡器的 負載平衡負載的多工器。  
  
### <a name="hosts-that-are-running-hyper-v"></a>執行 HYPER-V 的主機  
您可以使用 SLB，與執行 Windows Server 2016 和 HYPER-V 的電腦。 HYPER-V 主機上的 Vm 可以執行任何 HYPER-V 所支援的作業系統。  
  
### <a name="slb-host-agent"></a>SLB 主機代理程式  
當您部署 SLB 時，則您必須使用 System Center、 Windows PowerShell 或其他管理應用程式，來部署 SLB 主機代理程式，每個 HYPER-V 主機電腦上。 您可以在 Windows Server 2016 的 HYPER-V 支援，包括 Nano 伺服器的所有版本上安裝 SLB 主機代理程式。  
  
SLB 主機代理程式會接聽從網路控制站的 SLB 原則更新。 此外，主機代理程式的程式規則的 SLB 到 SDN 啟用 HYPER-V 虛擬交換器設定本機電腦上。  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN 啟用 HYPER-V 虛擬交換器  
SLB，相容的虛擬交換器的您必須使用 HYPER-V 虛擬交換器管理員] 或 [Windows PowerShell 命令來建立交換器，然後再您必須啟用虛擬篩選平台 (VFP) 虛擬交換器。  
  
在 虛擬交換器上啟用 VFP 的資訊，請參閱 Windows PowerShell 命令[Get-vmsystemswitchextension](https://technet.microsoft.com/library/hh848603.aspx)並[Enable-vmswitchextension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396)。  
  
SDN 啟用 HYPER-V 虛擬交換器的 SLB 執行下列動作。  
  
-   SLB 處理的資料路徑。  
  
-   從 MUX 接收輸入的網路流量。  
  
-   會略過輸出網路流量，將它傳送給 DSR 路由器的 MUX。  
  
-   在 Nano Server 執行個體的 HYPER-V 上執行。  
  
### <a name="bgp-enabled-router"></a>已啟用 BGP 的路由器  
BGP 路由器的 SLB 執行下列動作。  
  
-   若要使用 ECMP MUX 輸入的流量的路由。  
  
-   對於輸出網路流量，會使用主應用程式所提供的路由。  
  
-   會接聽路由更新從 SLB MUX 的 Vip。  
  
-   如果保持運作失敗，則您可以移除 SLB 旋轉 SLB Mux。  
  
## <a name="bkmk_features"></a>軟體負載平衡功能  
以下是一些功能和 SLB 的功能。  
  
**核心功能**  
  
-   SLB 會提供第 4 層負載平衡 '北-南' 和 '東-西' TCP/UDP 流量的服務  
  
-   您可以使用 HYPER-V 網路虛擬化型網路上的 SLB  
  
-   您可以使用 SLB 與 VLAN 型網路，適用於 DIP 的 Vm 連線至 SDN 啟用 HYPER-V 虛擬交換器。  
  
-   一個的 SLB 執行個體可以處理多個租用戶  
  
-   SLB 和 DIP 支援可調整且低延遲的傳回路徑，因為實作由 Direct Server Return (DSR)  
  
-   您也會使用 Switch Embedded Teaming (SET) 或單一根目錄輸入/輸出虛擬化 (SR-IOV) 時的 SLB 函式  
  
-   SLB 包含網際網路通訊協定第 4 版 (IPv4) 支援  
  
-   站對站閘道的情況下，SLB 提供 NAT 功能，可讓所有的站對站連線，以利用單一公用 IP  
  
-   您可以安裝 SLB，包括完整的主機代理程式和 MUX，Windows Server 2016，、 Core 和 Nano 安裝。  
  
**規模和效能**  
  
-   雲端規模，包括向外延展功能，可供與多工器和主機代理程式擴充功能。  
  
-   一個使用中的 SLB 管理員網路控制器模組可支援 8 個 MUX 執行個體  
  
**高可用性**  
  
-   您可以將 SLB 部署至主動/主動組態中的 2 個以上節點  
  
-   多工器可以新增或移除 MUX 集區，而不會影響的 SLB 服務。 這樣會維護 SLB 可用性時   
    正在修補個別的多工器。  
  
-   個別的 MUX 執行個體都有 99%的執行時間  
  
-   健全狀況監視資料可供管理實體  
  
**對齊方式**  
  
-   您可以部署和設定 SCVMM SLB  
  
-   SLB 會提供多租用戶的統一的邊緣，順暢地整合與 Microsoft 設備，例如 RAS 多租用戶閘道、 資料中心防火牆和路由反映程式。  
  

