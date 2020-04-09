---
title: SDN 的軟體負載平衡 (SLB)
description: 您可以使用本主題來瞭解 Windows Server 2016 中軟體定義網路的軟體負載平衡。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 245faa00eed6f8ee49f8d178ab2cde5d01b1e23e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859571"
---
# <a name="software-load-balancing-slb-for-sdn"></a>適用于 SDN 的軟體負載平衡 \(SLB\)

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題來瞭解 Windows Server 2016 中軟體定義網路的軟體負載平衡。  

在 Windows Server 2016 中部署軟體定義網路（SDN）的雲端服務提供者（Csp）和企業可以使用軟體負載平衡（SLB），將租使用者和租使用者客戶網路流量平均分散到虛擬網路資源。 Windows Server SLB 讓多部伺服器能夠裝載相同的工作負載，並提供高度可用性和延展性。
  
Windows Server SLB 包含下列功能。  
  
-   第4層（L4）為「北歐」和「美國東部」 TCP/UDP 流量的負載平衡服務。  
  
-   公用和內部網路流量負載平衡。  
  
-   支援虛擬區域網路（Vlan）和您使用 Hyper-v 網路虛擬化所建立之虛擬網路上的動態 IP 位址（Dip）。  
  
-   健康情況探查支援。  
  
-   準備好進行雲端規模，包括向外延展功能，以及 multiplexers (和主機代理程式的相應增加功能。  
  
如需詳細資訊，請參閱本主題中的[軟體負載平衡功能](#bkmk_features)。  
  
> [!NOTE]  
> 網路控制卡不支援 Vlan 的多租使用者，不過您可以針對服務提供者管理的工作負載（例如資料中心基礎結構和高密度 Web 服務器）使用具有 SLB 的 Vlan。  
  
使用 Windows Server SLB，您可以在您用於其他 VM 工作負載的相同 Hyper-v 計算伺服器上，使用 SLB Vm 來相應放大您的負載平衡功能。 因此，SLB 支援快速建立和刪除 CSP 作業所需的負載平衡端點。 此外，Windows Server SLB 支援每個叢集數十 gb、提供簡單的布建模型，而且很容易相應放大和縮小。  
  
**SLB 的運作方式**  
  
SLB 的運作方式是將虛擬 IP 位址（Vip）對應到屬於資料中心內雲端服務集合之一部分的動態 IP 位址（Dip）。  
  
Vip 是單一 IP 位址，可提供負載平衡 Vm 集區的公用存取權。 例如，Vip 是在網際網路上公開的 IP 位址，讓租使用者和租使用者客戶可以連線到雲端資料中心內的租使用者資源。  
  
Dip 是在 VIP 後方的負載平衡集區之成員 Vm 的 IP 位址。 Dip 會指派給租使用者資源的雲端基礎結構內。  
  
Vip 位於 SLB 多工器（MUX）中。  MUX 包含一或多部虛擬機器（Vm）。  網路控制站會為每個 VIP 提供每個 MUX，而每個 MUX 接著會使用邊界閘道協定（BGP），將每個 VIP 公告至實體網路上的路由器作為/32 路由。  BGP 允許實體網路路由器：  
  
-   瞭解每個 MUX 上都有可用的 VIP，即使 Mux 位於第3層網路中的不同子網也一樣。  
  
-   使用相同的成本多重路徑（ECMP）路由，將每個 VIP 的負載分散到所有可用的 Mux。  
  
-   自動偵測 MUX 失敗或移除，並停止將流量傳送到失敗的 MUX。  
  
-   在狀況良好的 Mux 中，將負載從失敗或移除的 MUX 分散。  
  
當公用流量從網際網路抵達時，SLB MUX 會檢查流量（其中包含 VIP 作為目的地），然後對應並重寫流量，使其抵達個別的 DIP。 對於輸入網路流量，此交易會在兩個步驟的程式中執行，該進程會在 MUX 虛擬機器（Vm）與目的地 DIP 所在的 Hyper-v 主機之間分割：  
  
-   負載平衡-MUX 會使用 VIP 來選取 DIP、封裝封包，並將流量轉送至 DIP 所在的 Hyper-v 主機。  
  
-   網路位址轉譯（NAT）-Hyper-v 主機會從封包移除封裝、將 VIP 轉譯為 DIP、重新對應埠，然後將封包轉送至 DIP VM。  
  
MUX 知道如何將 Vip 對應到正確的 Dip，因為您使用網路控制站定義的負載平衡原則。 這些規則包括通訊協定、前端埠、後端埠，以及散發演算法（5、3或2個元組）。  
  
當租使用者 Vm 回應並將輸出網路流量傳送回網際網路或遠端租使用者位置時，因為 NAT 是由 Hyper-v 主機執行，所以流量會略過 MUX，並從 Hyper-v 主機直接前往邊緣路由器。 此 MUX 略過程式稱為 Direct Server Return （DSR）。  
  
建立初始網路流量之後，輸入網路流量就會完全略過 SLB MUX。  
  
在下圖中，用戶端電腦會針對公司 SharePoint 網站的 IP 位址執行 DNS 查詢-在此案例中，是名為 Contoso 的虛構公司。 會發生下列進程。  
  
-   DNS 伺服器會將 VIP 107.105.47.60 傳回給用戶端。  
  
-   用戶端會將 HTTP 要求傳送至 VIP。  
  
-   實體網路有多個路徑可供連線到位於任何 MUX 上的 VIP。  沿著這種方式，每個路由器都會使用 ECMP 來挑選路徑的下一個區段，直到要求抵達 MUX 為止。  
  
-   接收要求的 MUX 會檢查已設定的原則，並看到虛擬網路上有兩個可用的 Dip （10.10.10.5 和10.10.20.5），以處理對 VIP 107.105.47.60 的要求。  
  
-   MUX 會選取 DIP 10.10.10.5，並使用 VXLAN 封裝封包，使其可以使用主機實體網路位址將它傳送至包含 DIP 的主機。  
  
-   主機會接收封裝的封包並加以檢查。  它會移除封裝並重寫封包，讓目的地現在是 DIP 10.10.10.5，而不是 VIP，並將流量傳送至 DIP VM。  
  
-   要求現在已到達伺服器陣列2中的 Contoso SharePoint 網站。 伺服器會產生回應，並使用自己的 IP 位址做為來源，將它傳送至用戶端。  
  
-   主機會攔截虛擬交換器中的傳出封包，這會記住用戶端（現在是目的地）已對 VIP 提出原始要求。  主機會將封包的來源重寫為 VIP，讓用戶端看不到 DIP 位址。  
  
-   主機會將封包直接轉送至實體網路的預設閘道，而該閘道會使用其標準路由表將封包轉送到最後接收回應的用戶端。  
  
![軟體負載平衡程式](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**負載平衡內部資料中心流量**  
  
當負載平衡資料中心內部的網路流量（例如，在不同伺服器上執行的租使用者資源，以及相同虛擬網路的成員）時，Vm 連線的 Hyper-v 虛擬交換器會執行 NAT。  
  
使用內部流量負載平衡時，第一個要求會傳送至 MUX 並由 MUX 處理，這會選取適當的 DIP 並將流量路由傳送至 DIP。 從該時間點開始，已建立的流量會略過 MUX，並直接從 VM 移至 VM。  
  
**健康情況探查**  
  
SLB 包含健康情況探查，可驗證網路基礎結構的健康情況，包括下列各項。  
  
-   TCP 探查至埠  
  
-   HTTP 探查至埠和 URL  
  
不同于傳統負載平衡器設備，其中探查是在應用裝置上進行，並透過網路傳輸至 DIP，SLB 探查會在 DIP 所在的主機上產生，並直接從 SLB 主機代理程式傳送至 DIP，進一步發佈跨主機工作。  
  
## <a name="software-load-balancing-infrastructure"></a><a name="bkmk_infrastructure"></a>軟體負載平衡基礎結構  
若要部署 Windows Server SLB，您必須先在 Windows Server 2016 中部署網路控制站，以及一或多個 SLB MUX Vm。  
  
此外，您必須使用已啟用 SDN 的 Hyper-v 虛擬交換器來設定 Hyper-v 主機，並確定 SLB 主機代理程式正在執行。  服務主機的路由器必須支援相等的成本多重路徑（ECMP）路由和邊界閘道協定（BGP），而且必須設定為接受 SLB Mux 的 BGP 對等互連要求。  
  
以下是 SLB 基礎結構的總覽。  

![軟體負載平衡基礎結構](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
下列各節提供 SLB 基礎結構的這些元素的詳細資訊。  
  
### <a name="scvmm"></a>SCVMM  
使用 System Center 2016，您可以在 Windows Server 2016 上設定網路控制站，包括 SLB 管理員和健全狀況監視。 您也可以使用 System Center 來部署 SLB MUXs，以及在執行 Windows Server 2016 和 Hyper-v 的電腦上安裝 SLB 主機代理程式。  
  
如需 System Center 2016 的詳細資訊，請參閱[System center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/)。  
  
> [!NOTE]  
> 如果您不想要使用 System Center 2016，您可以使用 Windows PowerShell 或其他管理應用程式來安裝和設定網路控制站和其他 SLB 基礎結構。 如需詳細資訊，請參閱[使用 Windows PowerShell 部署網路控制](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)站。  
  
### <a name="network-controller"></a>網路控制卡  
網路控制卡會裝載 SLB 管理員，並針對 SLB 執行下列動作。  
  
-   處理透過 Northbound API 從 System Center、Windows PowerShell 或其他網路管理應用程式傳入的 SLB 命令。  
  
-   計算散發至 Hyper-v 主機和 SLB Mux 的原則。  
  
-   提供 SLB 基礎結構的健全狀況狀態。  
  
### <a name="slb-mux"></a>SLB MUX  
SLB MUX 會處理輸入網路流量，並將 Vip 對應至 Dip，然後將流量轉送至正確的 DIP。 每個 MUX 也會使用 BGP 將 VIP 路由發佈到邊緣路由器。 當 MUX 失敗時，BGP Keep-alive 會通知 Mux，這可讓作用中的 Mux 在發生 MUX 失敗時重新發佈負載，基本上是提供負載平衡器的負載平衡。  
  
### <a name="hosts-that-are-running-hyper-v"></a>執行 Hyper-v 的主機  
您可以使用 SLB 搭配執行 Windows Server 2016 和 Hyper-v 的電腦。 Hyper-v 主機上的 Vm 可以執行 Hyper-v 所支援的任何作業系統。  
  
### <a name="slb-host-agent"></a>SLB 主機代理程式  
當您部署 SLB 時，您必須使用 System Center、Windows PowerShell 或其他管理應用程式，在每部 Hyper-v 主機電腦上部署 SLB 主機代理程式。 您可以在提供 Hyper-v 支援的所有 Windows Server 2016 版本（包括 Nano 伺服器）上安裝 SLB 主機代理程式。  
  
SLB 主機代理程式會從網路控制卡接聽 SLB 原則更新。 此外，主機代理程式會在本機電腦上設定為啟用 SDN 的 Hyper-v 虛擬交換器中的 SLB 規則。  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>已啟用 SDN 的 Hyper-v 虛擬交換器  
若要讓虛擬交換器與 SLB 相容，您必須使用 Hyper-v 虛擬交換器管理員或 Windows PowerShell 命令來建立交換器，然後您必須啟用虛擬交換器的虛擬篩選平臺（VFP）。  
  
如需在虛擬交換器上啟用 VFP 的詳細資訊，請參閱 Windows PowerShell 命令[get-vmsystemswitchextension](https://technet.microsoft.com/library/hh848603.aspx)和[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396)。  
  
SDN 已啟用的 Hyper-v 虛擬交換器會針對 SLB 執行下列動作。  
  
-   處理 SLB 的資料路徑。  
  
-   接收來自 MUX 的輸入網路流量。  
  
-   略過輸出網路流量的 MUX，使用 DSR 將它傳送至路由器。  
  
-   在 Hyper-v 的 Nano Server 實例上執行。  
  
### <a name="bgp-enabled-router"></a>已啟用 BGP 的路由器  
BGP 路由器會針對 SLB 執行下列動作。  
  
-   使用 ECMP 將輸入流量路由傳送至 MUX。  
  
-   對於輸出網路流量，會使用主機所提供的路由。  
  
-   從 SLB MUX 接聽 Vip 的路由更新。  
  
-   如果保持運作失敗，則從 SLB 旋轉移除 SLB Mux。  
  
## <a name="software-load-balancing-features"></a><a name="bkmk_features"></a>軟體負載平衡功能  
以下是 SLB 的部分特性和功能。  
  
**核心功能**  
  
-   SLB 為「北-南部」和「美國東部」 TCP/UDP 流量提供第4層負載平衡服務  
  
-   您可以在 Hyper-v 網路虛擬化為基礎的網路上使用 SLB  
  
-   您可以針對連線到已啟用 SDN 的 Hyper-v 虛擬交換器的 DIP Vm，使用 SLB 搭配 VLAN 型網路。  
  
-   一個 SLB 實例可以處理多個租使用者  
  
-   SLB 和 DIP 支援可調整且低延遲的傳回路徑，如同由 Direct Server Return （DSR）所實行  
  
-   當您也使用交換器內嵌小組（SET）或單一根目錄輸入/輸出虛擬化（SR-IOV）時的 SLB 函數  
  
-   SLB 包含網際網路通訊協定第4版（IPv4）支援  
  
-   針對站對站閘道案例，SLB 提供 NAT 功能，可讓所有站對站連線使用單一公用 IP  
  
-   您可以在 Windows Server 2016、完整版、核心版和 Nano 安裝上安裝 SLB，包括主機代理程式和 MUX。  
  
**規模與效能**  
  
-   準備好進行雲端規模，包括向外延展功能，以及 Mux 和主機代理程式的相應增加功能。  
  
-   一個有效的 SLB 管理員網路控制卡模組可支援8個 MUX 實例  
  
**高可用性**  
  
-   在主動/主動設定中，您可以將 SLB 部署到2個以上的節點  
  
-   Mux 可以從 MUX 集區新增和移除，而不會影響 SLB 服務。 這會在下列情況下維護 SLB 可用性   
    正在修補個別 Mux。  
  
-   個別的 MUX 實例具有99% 的執行時間  
  
-   健全狀況監視資料可供管理實體使用  
  
**對齊**  
  
-   您可以使用 SCVMM 部署和設定 SLB  
  
-   SLB 藉由與 Microsoft 應用裝置緊密整合，例如 RAS 多租使用者閘道、資料中心防火牆和路由反映程式，提供多租使用者整合的優勢。  
  

