---
title: Windows Server 2016 中的 hyper-v 網路虛擬化技術詳細資料
description: 本主題提供有關 Windows Server 2016 中 Hyper-v 網路虛擬化的技術資訊
manager: grcusanz
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 94d45dd04708d58d880f3dcbc7a65e9586e02a29
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994406"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Windows Server 2016 中的 hyper-v 網路虛擬化技術詳細資料

>適用于： Windows Server 2016

伺服器虛擬化可以讓多個伺服器執行個體同時在單一實體主機上執行；且伺服器執行個體還可以彼此獨立。 每部虛擬機器的基本操作方式就像是實體電腦上唯一執行的伺服器一樣。

網路虛擬化提供類似的功能，其中有多個虛擬網路 (有重迭的 IP 位址，) 在相同的實體網路基礎結構上執行，而每個虛擬網路的運作方式，就像是在共用網路基礎結構上唯一執行的虛擬網路一樣。 圖 1 顯示此關係。

![伺服器虛擬化與網路虛擬化](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)

圖 1：伺服器虛擬化與網路虛擬化

## <a name="hyper-v-network-virtualization-concepts"></a>Hyper-V 網路虛擬化概念
在 Hyper-v 網路虛擬化 (HNV) 中，客戶或租使用者會定義為部署在企業或資料中心的一組 IP 子網的「擁有者」。 客戶可以是公司或企業，在需要網路隔離的私人資料中心，或在服務提供者所裝載的公用資料中心內有多個部門或業務單位。 每個客戶都可以在資料中心內有一或多個[虛擬網路](#VirtualNetworks)，而每個虛擬網路都包含一或多個[虛擬子網](#VirtualSubnets)。

Windows Server 2016 中有兩種 HNV 的實現方式： HNVv1 和 HNVv2。

-   **HNVv1**

    HNVv1 與 Windows Server 2012 R2 和 System Center 2012 R2 Virtual Machine Manager (VMM) 相容。 HNVv1 的設定依賴 WMI 管理和 Windows PowerShell Cmdlet (透過 System Center VMM) 來定義隔離設定和客戶位址 (CA) -虛擬網路到實體位址 (PA) 對應和路由。 Windows Server 2016 中的 HNVv1 沒有新增任何功能，而且沒有規劃任何新功能。

    *   設定小組和 HNV V1 與平臺不相容。

    o 若要使用 HA NVGRE 閘道，使用者必須使用 LBFO 小組或不是小組。 或者

    o 使用已部署的網路控制站閘道搭配設定的組合參數。


-   **HNVv2**

    HNVv2 中包含大量的新功能，這是使用 Azure 虛擬篩選平臺在 Hyper-v 交換器中 (VFP) 轉送延伸模組來執行。 HNVv2 已與 Microsoft Azure Stack 完全整合，其中包含軟體定義網路 (SDN) 堆疊中的新網路控制站。  虛擬網路原則是透過 Microsoft[網路控制](../../../sdn/technologies/network-controller/Network-Controller.md)卡，使用 RESTful NORTHBOUND (NB) API，並藉由多個 SouthBound INTEFACES (為) （包括 OVSDB）向主機代理程式進行檢測。 主機代理程式會在其強制執行所在的 Hyper-v 交換器的 VFP 延伸模組中套用原則。

    > [!IMPORTANT]
    > 本主題著重于 HNVv2。

### <a name="virtual-network"></a><a name="VirtualNetworks"></a>虛擬網路

-   每個虛擬網路都是由一或多個虛擬子網所組成。 虛擬網路會形成隔離界限，其中虛擬網路內的虛擬機器只能彼此通訊。 傳統上，這種隔離是使用具有隔離 IP 位址範圍和 802.1 q 標記或 VLAN ID 的 Vlan 來強制執行。 但使用 HNV 時，會使用 NVGRE 或 VXLAN 封裝來強制執行隔離，以建立重迭網路，而且可能會在客戶或租使用者之間有重迭的 IP 子網。

-   每個虛擬網路在主機上都有唯一的路由網域識別碼 (RDID) 。 此 RDID 大約會對應至資源識別碼，以識別網路控制卡中的虛擬網路 REST 資源。 虛擬網路 REST 資源是使用統一資源識別項來參考， (URI) 命名空間加上附加的資源識別碼。

### <a name="virtual-subnets"></a><a name="VirtualSubnets"></a>虛擬子網路

-   虛擬子網路會針對相同虛擬子網路中的虛擬機器，實作第 3 層 IP 子網路語意。 虛擬子網會形成一個廣播網域 (類似于 VLAN) 並會使用 NVGRE 租使用者網路識別碼 (TNI) 或 VXLAN Network Identifier (VNI) 欄位來強制執行隔離。

-   每個虛擬子網都屬於一個 (RDID) 的單一虛擬網路，而且系統會為其指派唯一的虛擬子網識別碼 (VSID) 使用封裝的封包標頭中的 TNI 或 VNI 金鑰。 VSID 在資料中心內必須是唯一的，且範圍介於4096到 2 ^ 24-2 之間。

虛擬網路和路由網域的主要優點是，它可讓客戶攜帶自己的網路拓撲 (例如，將 IP 子網) 到雲端。 圖 2 顯示的範例是 Contoso 公司擁有兩個獨立的網路：R&D 網路和銷售網路。 由於這兩個網路具有不同的路由網域識別碼，因此彼此無法互動。 也就是說，Contoso R&D Net 會與 Contoso Sales Net 隔離，即使這兩者都是由 Contoso Corp 所擁有。 Contoso R&D Net 包含三個虛擬子網。 請注意，RDID 和 VSID 在資料中心內都是唯一的。

![客戶網路和虛擬子網路](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)

圖 2：客戶網路和虛擬子網路

**第2層轉送**

在 [圖 2] 中，VSID 5001 中的虛擬機器可以透過 Hyper-v 交換器將其封包轉送到也在 VSID 5001 中的虛擬機器。 來自 VSID 5001 中虛擬機器的連入封包，會傳送至 Hyper-v 交換器上的特定 VPort。 輸入規則 (例如 encap) 和對應 (例如封裝標頭) 會由 Hyper-v 交換器針對這些封包套用。 封包接著會轉送到 Hyper-v 交換器上的不同 VPort (如果目的地虛擬機器連接到相同的主機) 或不同主機上的不同 Hyper-v 交換器 (如果目的地虛擬機器位於不同的主機) 上。

**第3層路由**

同樣地，VSID 5001 中的虛擬機器可以將其封包路由傳送至 VSID 5002 或 VSID 5003 中的虛擬機器，而 HNV 分散式路由器會出現在每個 Hyper-v 主機的 VSwitch 中。 將封包傳遞至 Hyper-v 交換器時，HNV 會將傳入封包的 VSID 更新為目的地虛擬機器的 VSID。 只有當這兩個 VSID 位於相同 RDID 時才會發生此情況。  因此，具有 RDID1 的虛擬網路介面卡無法使用 RDID2 將封包傳送到虛擬網路介面卡，而不需要遍歷閘道。

> [!NOTE]
> 在上述的封包流程描述中，「虛擬機器」一詞實際上表示虛擬機器上的虛擬網路介面卡。 常見情況是虛擬機器只會有單一虛擬網路介面卡。 在此情況下，「虛擬機器」和「虛擬網路介面卡」一詞在概念上也是相同的意義。

每個虛擬子網路都會定義第 3 層 IP 子網路，而第 2 層 (L2) 廣播網域界限會與 VLAN 類似。 當虛擬機器廣播封包時，HNV 會使用單播複寫 (UR) 複製原始封包，並將目的地 IP 和 MAC 取代為相同 VSID 中每個 VM 的位址。

> [!NOTE]
> 當 Windows Server 2016 發行時，將會使用單播複寫來執行廣播和子網多播。 不支援跨子網多播路由和 IGMP。

除了做為廣播網域之外，VSID 還提供隔離功能。 HNV 中的虛擬網路介面卡會連線至 Hyper-v 交換器埠，其會將 ACL 規則直接套用到埠 (virtualNetworkInterface REST 資源) 或其所屬的虛擬子網 (VSID) 。

Hyper-v 交換器埠必須套用 ACL 規則。 此 ACL 可能會允許全部、全部拒絕或更明確，僅允許以5個元組為基礎的特定類型流量， (來源 IP、目的地 IP、來源埠、目的地埠、通訊協定) 比對。

> [!NOTE]
> Hyper-v 交換器擴充功能將無法在新的軟體定義網路 (SDN) 堆疊中使用 HNVv2。 HNVv2 是使用 Azure 虛擬篩選平臺來執行， (VFP) 交換器擴充功能，這不能與其他任何協力廠商交換器擴充功能一起使用。

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Hyper-v 網路虛擬化中的切換和路由
HNVv2 會執行正確的第2層 (L2) 切換和第3層 (L3) 路由語義，使其如同實體交換器或路由器可正常操作。 當連線到 HNV 虛擬網路的虛擬機器嘗試與相同虛擬子網中的另一部虛擬機器建立連接時 (VSID) 必須先瞭解遠端虛擬機器的 CA MAC 位址。 如果來源虛擬機器的 ARP 資料表中有目的地虛擬機器 IP 位址的 ARP 專案，則會使用此專案中的 MAC 位址。 如果專案不存在，則來源虛擬機器將會傳送 ARP 廣播，其中包含對應至目的地虛擬機器 IP 位址的 MAC 位址要求。 Hyper-v 交換器會攔截此要求，並將它傳送至主機代理程式。 主機代理程式會在其本機資料庫中，尋找所要求目的地虛擬機器 IP 位址的對應 MAC 位址。

> [!NOTE]
> 作為 OVSDB 伺服器的主機代理程式會使用 VTEP 架構的變體來儲存 CA PA 對應、MAC 資料表等等。

如果 MAC 位址可供使用，則主機代理程式會插入 ARP 回應，並將其傳回給虛擬機器。 虛擬機器的網路堆疊具有所有必要的 L2 標頭資訊之後，畫面會傳送至 V 交換器上對應的 Hyper-v 埠。 就內部而言，Hyper-v 交換器會針對指派給 V 通訊埠的 N 元組比對規則來測試此框架，並根據這些規則將特定的轉換套用至框架。 最重要的是，會套用一組封裝轉換，根據網路控制站上定義的原則，使用 NVGRE 或 VXLAN 來建立封裝標頭。 根據主機代理程式所編寫的原則，會使用 CA PA 對應來判斷目的地虛擬機器所在之 Hyper-v 主機的 IP 位址。 Hyper-v 交換器可確保正確的路由規則和 VLAN 標記會套用至外部封包，使其到達遠端 PA 位址。

如果連線到 HNV 虛擬網路的虛擬機器想要與不同虛擬子網中的虛擬機器建立連接 (VSID) ，封包必須據以路由傳送。 HNV 會假設在 CA 空間中只有一個 IP 位址是用來做為下一個躍點來到達所有 IP 首碼的星號拓撲， (表示一個預設路由/閘道) 。 目前，這會強制執行單一預設路由的限制，而非預設路由則不受支援。

### <a name="routing-between-virtual-subnets"></a>在虛擬子網路間路由
在實體網路中，IP 子網是第2層 (L2) 網域，其中的電腦 (虛擬和實體) 可以直接彼此通訊。 L2 網域是一個廣播網域，其中的 ARP 專案 (IP： MAC 位址對應) 透過在所有介面廣播的 ARP 要求來學習，而 ARP 回應則會傳回給要求的主機。 電腦會使用從 ARP 回應中學習到的 MAC 資訊來完整地構造 L2 框架，包括 Ethernet 標頭。 不過，如果 IP 位址位於不同的 L3 子網中，則 ARP 要求不會跨越此 L3 界限。 相反地，L3 路由器介面 (下一個躍點或預設閘道) ，而來源子網中的 IP 位址必須以自己的 MAC 位址回應這些 ARP 要求。

在標準 Windows 網路中，系統管理員可以建立靜態路由，並將它們指派給網路介面。 此外，「預設閘道」通常會設定為介面上的下一個躍點 IP 位址，而在此介面上，會傳送目的地為預設路由 (0.0.0.0/0) 的封包。 如果沒有特定的路由存在，封包就會傳送到這個預設閘道。 這通常是實體網路的路由器。  HNV 使用屬於每部主機的內建路由器，並在每個 VSID 中具有介面，以建立虛擬網路 (s) 的分散式路由器。

由於 HNV 採用星狀拓撲，因此 HNV 分散式路由器會作為在屬於相同 VSID 網路之一部分之虛擬子網之間的所有流量的單一預設閘道。 做為預設閘道的位址預設為 VSID 中的最低 IP 位址，並會指派給 HNV 分散式路由器。 此分散式路由器允許非常有效率的方式，讓 VSID 網路內的所有流量適當地路由傳送，因為每一部主機都可以直接將流量路由傳送至適當的主機，而不需要媒介。  當相同的實體主機上的兩部虛擬機器位於相同 VM 網路，但不同虛擬子網路時，這非常有用。  您將會在本節稍後看到，封包完全不需要離開實體主機。

### <a name="routing-between-pa-subnets"></a>PA 子網之間的路由
相較于為每個虛擬子網配置一個 PA IP 位址的 HNVv1 (VSID) ，HNVv2 現在會針對每個交換器內嵌小組使用一個 PA IP 位址， (設定) NIC 小組成員。 預設部署假設有兩個 NIC 的小組，並為每個主機指派兩個 PA IP 位址。 單一主機具有從相同的提供者指派的 PA Ip， (PA) 相同 VLAN 上的邏輯子網。 相同虛擬子網中的兩個租使用者 Vm 可能確實位於連線到兩個不同提供者邏輯子網的兩部不同主機上。 HNV 會根據 CA-PA 對應來為封裝的封包，建立外部 IP 標頭。 不過，它會依賴主機 TCP/IP 堆疊到預設 PA 閘道的 ARP，然後根據 ARP 回應來建立外部乙太網路標頭。 一般來說，此 ARP 回應是來自實體交換器或主機連線的 L3 路由器上的 SVI 介面。 因此，HNV 會依賴 L3 路由器來路由提供者邏輯子網/Vlan 之間封裝的封包。

### <a name="routing-outside-a-virtual-network"></a>在虛擬網路外路由
多數的客戶部署將需要從 HNV 環境與不屬於 HNV 環境的資源通訊。 這樣必須要有網路虛擬化閘道，才允許在這兩種環境之間通訊。 需要 HNV 閘道的基礎結構包括私用雲端和混合式雲端。 基本上，第3層路由需要 HNV 閘道，在內部和外部 (實體) 網路 (包括 NAT) 或不同網站和/或雲端之間， (使用 IPSec VPN 或 GRE 通道的私人或公用) 。

閘道可以有不同的實際規格。 它們可以建置於 Windows Server 2016 上，併入機架的最上層 (TOR) 交換器，可透過負載平衡器所通告的虛擬 IP (VIP) 、放入其他現有的網路應用裝置，或作為新的獨立網路應用裝置來進行存取。

如需 Windows RAS 閘道選項的詳細資訊，請參閱[RAS 閘道](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。

## <a name="packet-encapsulation"></a>封包壓縮
HNV 中的每個虛擬網路介面卡都會與下列兩個 IP 位址相關聯：

-   **客戶位址** (CA 會根據其內部網路基礎結構，) 客戶指派的 IP 位址。 此位址可讓客戶與虛擬機器切換式網路流量，就像它尚未移到公用或私用雲端一樣。 虛擬機器可以看見 CA，而客戶也可以連線至該 CA。

-   **提供者位址** (PA) 由主機服務提供者或資料中心系統管理員根據其實體網路基礎結構指派的 IP 位址。 PA 會出現在網路的封包中，這些封包會與執行裝載虛擬機器之 Hyper-V 的伺服器進行交換。 您可以在實體網路上看見 PA，但虛擬機器看不見該 PA。

CA 會保留客戶的網路拓樸，這樣會虛擬化並分隔實際基礎實體網路拓樸和位址，如同 PA 所實作。 下圖顯示網路虛擬化之後的虛擬機器 CA 和網路基礎結構 PA 之間的概念性關係。

![實體基礎結構網路虛擬化的概念圖](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)

圖 6：實體基礎結構的網路虛擬化概念性圖表

在此圖中，客戶虛擬機器會在 CA 空間中傳送資料封包，以透過自己的虛擬網路或「通道」來跨越實體網路基礎結構。 在上述範例中，您可以將通道視為 Contoso 和 Fabrikam 資料封包的「信封」，其中包含綠色的出貨標籤 (PA 位址) 要從左邊的來源主機傳遞至右邊的目的地主機。 金鑰是主機如何判斷「運送位址」 (PA 的) 與 Contoso 和 Fabrikam CA 的對應，以及如何將「信封」放在封包中，以及目的地主機如何解除包裝封包，以及如何正確地傳遞至 Contoso 和 Fabrikam 目的地虛擬機器。

這個簡單的比喻可以突顯網路虛擬化的重點概念：

-   每個虛擬機器 CA 都會對應到實體主機 PA。 而多個 CA 可以和同一 PA 建立關聯。

-   虛擬機器會傳送 CA 空間中的資料封包，並根據對應，將其放入具有 PA 來源和目的地配對的「信封」中。

-   CA-PA 對應必須能夠讓主機區分不同客戶虛擬機器的封包。

如此一來，將網路虛擬化的機制就是將虛擬機器使用的網路位址虛擬化。 網路控制站負責位址對應，而主機代理程式會使用 MS_VTEP 架構來維護對應資料庫。 下一節將說明實際的位址虛擬化機制。

## <a name="network-virtualization-through-address-virtualization"></a>透過位址虛擬化進行網路虛擬化
HNV 會使用網路虛擬化一般路由封裝 (NVGRE) 或虛擬可擴充的區域網路 (VXLAN) 來執行重迭租使用者網路。  VXLAN 是預設值。

### <a name="virtual-extensible-local-area-network-vxlan"></a>虛擬可擴充區域網路 (VXLAN) 
 (VXLAN)  ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) 通訊協定的虛擬可延伸區域網路，已在市場上廣泛採用，並提供來自 Cisco、織錦、Arista、DELL、HP 等廠商的支援。 VXLAN 通訊協定會使用 UDP 做為傳輸。 適用于 VXLAN 的 IANA 指派 UDP 目的地埠是4789，而 UDP 來源埠應該是要用於 ECMP 散佈之內部封包中的資訊雜湊。 在 UDP 標頭之後，會將 VXLAN 標頭附加至封包，其中包含保留的4位元組欄位，後面接著3個位元組的 VXLAN 網路識別碼欄位 (VNI) -VSID-後面接著另一個保留的1位元組欄位。 在 VXLAN 標頭之後，不會附加) 的原始 CA L2 框架 (沒有 CA Ethernet 框架的 FCS。

![VXLAN 封包標頭](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)

### <a name="generic-routing-encapsulation-nvgre"></a> (NVGRE) 的泛型路由封裝
此網路虛擬化機制會使用一般路由封裝 (NVGRE) 作為通道標頭的一部分。 在 NVGRE 中，虛擬機器的封包會封裝在另一個封包內。 這個新封包的標頭除了虛擬子網路識別碼之外，還擁有適當的來源與目的地 PA IP 位址，虛擬子網路識別碼儲存於 GRE 標頭的索引鍵欄位中，如圖 7 所示。

![NVGRE 封裝](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)

圖 7：網路虛擬化 - NVGRE 封裝

「虛擬子網識別碼」可讓主機識別任何指定封包的客戶虛擬機器，即使該 PA 和封包上的 CA 可能會重迭也一樣。 這樣可讓相同主機上的所有虛擬機器共用單一 PA，如圖 7 所示。

共用 PA 對網路延展性有很大的影響。 網路基礎結構需要知道的 IP 和 MAC 位址數目會大幅減少。 例如，如果每部終端主機平均擁有 30 部虛擬機器，則網路基礎結構需要知道的 IP 和 MAC 位址數目會降低 30 倍。封包中內嵌的虛擬子網路識別碼也能夠輕易地將封包相互關聯到實際的客戶。

Windows Server 2012 R2 的 PA 共用配置是每部主機每個 VSID 的一個 PA。 針對 Windows Server 2016，此配置為每個 NIC 小組成員一個 PA。

在 Windows Server 2016 和更新版本中，HNV 完全支援現成的 NVGRE 和 VXLAN;它不需要升級或購買新的網路硬體，例如 Nic (網路介面卡) 、交換器或路由器。 這是因為網路上的這些封包在 PA 空間中是一般的 IP 封包，與現今的網路基礎結構相容。  不過，若要取得最佳效能，請使用支援工作卸載的最新驅動程式所支援的 Nic。

## <a name="multi-tenant-deployment-example"></a>多租用戶部署範例
下圖顯示兩個客戶的範例部署，其中包含網路原則所定義的 CA PA 關聯性的雲端資料中心。

![多租用戶部署範例](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)

圖 8：多組織用戶共享部署範例

討論圖 8 中的範例。 移到裝載提供者的共用 IaaS 服務之前：

-   Contoso Corp 會在 IP 位址 10.1.1.11 上執行 SQL Server (名為 **SQL**)，在 IP 位址 10.1.1.12 上執行網頁伺服器 (名為 **Web**) 並使用 SQL Server 進行資料庫交易。

-   Fabrikam Corp 會執行 SQL Server (也稱為 **SQL**) 並指派 IP 位址 10.1.1.11，以及執行網頁伺服器 (也稱為 **Web**)，IP 位址為 10.1.1.12 並使用 SQL Server 進行資料庫交易。

我們會假設主機服務提供者先前已建立提供者 (PA 透過網路控制站) 邏輯網路，以對應至其實體網路拓撲。 網路控制站會從邏輯子網的 IP 首碼（主機連線的位置）配置兩個 PA IP 位址。 網路控制卡也會指出適當的 VLAN 標記以套用 IP 位址。

使用網路控制站、Contoso Corp 和 Fabrikam Corp，然後建立其虛擬網路和子網（由提供者支援） (PA) 由主機服務提供者指定的邏輯網路。 Contoso Corp 和 Fabrikam Corp 都各自將 SQL Server 和網頁伺服器移到相同裝載提供者的共用 IaaS 服務，且剛好都在 Hyper-V 主機 1 上執行 **SQL** 虛擬機器，以及在 Hyper-V 主機 2 上執行 **Web** (IIS7) 虛擬機器。 所有虛擬機器都會保留原始的內部網路 IP 位址 (它們的 CA)。

這兩家公司都會獲指派下列虛擬子網識別碼 (VSID 由網路控制站) ，如下所示。  每個 Hyper-v 主機上的主機代理程式會從網路控制站接收配置的 PA IP 位址，並在非預設的網路區間中建立兩個 PA 主機 Vnic。 網路介面會指派給每一個指派 PA IP 位址的主機 Vnic，如下所示：

-   Contoso Corp 的虛擬機器 VSID 和 PAs： **VSID**為5001， **SQL pa**為192.168.1.10， **Web pa**為192.168.2.20

-   Fabrikam Corp 的虛擬機器 VSID 和 PAs： **VSID**為6001， **SQL pa**為192.168.1.10， **Web pa**為192.168.2.20

網路控制站會連接所有網路原則 (包括) 到 SDN 主機代理程式的 CA PA 對應，這會在) 的 OVSDB 資料庫資料表中維護持續性存放區 (中的原則。

當 Contoso Corp Web 虛擬機器 (10.1.1.12) 在 Hyper-v 主機2上建立與 SQL Server 的 TCP 連線時，將會發生下列情況：

-   10.1.1.11 目的地 MAC 位址的 VM Arp

-   VSwitch 中的 VFP 延伸模組會攔截此封包，並將它傳送至 SDN 主機代理程式

-   SDN 主機代理程式會在其原則存放區中尋找10.1.1.11 的 MAC 位址

-   如果找到 MAC，則主機代理程式會將 ARP 回應插入回 VM

-   如果找不到 MAC，就不會傳送任何回應，而且 VM 中的 ARP 專案會標示為無法連線。（10.1.1.11）

-   VM 現在會使用正確的 CA 乙太網路和 IP 標頭來建立 TCP 封包，並將其傳送至 vSwitch

-   VSwitch 中的 VFP 轉送延伸模組會透過 VFP 層處理此封包， (如下所述) 指派給接收封包的來源 vSwitch 埠，並在 VFP 整合流程資料表中建立新的流程專案

-   VFP 引擎會針對每個層 (執行規則比對或流程資料表查閱，例如，根據 IP 和乙太網路標頭) 的虛擬網路層。

-   虛擬網路層中的相符規則會參考 CA PA 對應空間，並執行封裝。

-   封裝類型 (VXLAN 或 NVGRE) 會在 VNet 層中與 VSID 一併指定。

-   在 VXLAN 封裝的情況下，會在 VXLAN 標頭中以5001的 VSID 來建立外部 UDP 標頭。
    外部 IP 標頭會使用指派給 Hyper-v 主機 2 (192.168.2.20) 和 Hyper-v 主機 1 (192.168.1.10) 的來源和目的地 PA 位址，分別以 SDN 主機代理程式的原則存放區為基礎。

-   此封包接著會流入 VFP 中的 PA 路由層。

-   VFP 中的 PA 路由層會參考用於 PA 空間流量和 VLAN 識別碼的網路區間，並使用主機的 TCP/IP 堆疊，正確地將 PA 封包轉送至 Hyper-v 主機1。

-   當收到封裝封包時，Hyper-v 主機1會接收 PA 網路區間中的封包，並將它轉送到 vSwitch。

-   VFP 會透過其 VFP 層處理封包，並在 VFP 整合流程資料表中建立新的流程專案。

-   VFP 引擎符合虛擬網路層中的 ingres 規則，並去除外部封裝的 Ethernet、IP 和 VXLAN 標頭。

-   然後，VFP 引擎會將封包轉送到目的地 VM 所連線的 vSwitch 埠。

Fabrikam Corp **Web** 和 **SQL** 虛擬機器會使用 Fabrikam Corp 的 HNV 原則設定執行類似的流量處理程序。如此一來，HNV、Fabrikam Corp 與 Contoso Corp 虛擬機器就可以進行互動，如同在原始內部網路一樣。 即使它們使用相同的 IP 位址，也絕對無法彼此互動。

 (Ca 和 PAs) 的個別位址、Hyper-v 主機的原則設定，以及輸入和輸出虛擬機器流量的 CA 與 PA 之間的位址轉譯，則會使用 NVGRE 金鑰或 VLXAN VNID 來隔離這些伺服器集合。 此外，虛擬化對應和轉換會分隔實體網路基礎架構與虛擬網路架構。 即使 Contoso **SQL** 和 **Web** 及 Fabrikam **SQL** 和 **Web** 都位於它們自己的 CA IP 子網路 (10.1.1/24) 內，其實體部署都發生在不同 PA 子網路 192.168.1/24 和 192.168.2/24 各自的主機上。 這暗示跨子網路的虛擬機器佈建和即時移轉可以透過 HNV 執行。

## <a name="hyper-v-network-virtualization-architecture"></a>Hyper-V 網路虛擬化架構
在 Windows Server 2016 中，HNVv2 是使用 Azure 虛擬篩選平臺來執行， (VFP) 這是 Hyper-v 交換器內的 NDIS 篩選延伸模組。 VFP 的重要概念是，符合動作流程引擎與內部 API 公開給 SDN 主機代理程式，以進行網路原則的設計。 SDN 主機代理程式本身會透過 OVSDB 和 WCF SouthBound 通道，從網路控制站接收網路原則。 虛擬網路原則不只 (例如，CA-PA 對應) 使用 VFP 進行設計，而其他原則（例如 Acl、QoS 等等）。

VSwitch 和 VFP 轉送擴充功能的物件階層如下：

-   vSwitch

    -   外部 NIC 管理

    -   NIC 硬體卸載

    -   全域轉送規則

    -   連接埠

        -   用來釘選的輸出轉送層

        -   對應和 NAT 集區的空間清單

        -   統一的流程資料表

        -   VFP 層

            -   Flow 資料表

            -   群組

            -   規則

                -   規則可以參考空格

在 VFP 中，會根據每個原則類型建立圖層 (例如虛擬網路) ，而且是一組一般的規則/流程資料表。 在特定規則指派給該層來執行這類功能之前，它不會有任何內建函式。 系統會為每個圖層指派優先順序，而層會依遞增優先順序指派給埠。 規則會組織成主要根據方向和 IP 位址系列的群組。 群組也會被指派優先順序，而且最多可以有一個來自群組的規則符合指定的流程。

具有 VFP 擴充功能之 vSwitch 的轉送邏輯如下所示：

-   輸入處理會從傳入埠的封包觀點來 (輸入) 

-   推行

-   從封包的觀點來看，輸出處理 (輸出離開埠) 

VFP 支援 NVGRE 和 VXLAN 封裝類型的內部 MAC 轉送，以及外部 MAC VLAN 的轉送。

VFP 延伸模組具有慢速路徑和快速路徑來進行封包傳送。 流程中的第一個封包必須跨越每一層中的所有規則群組，並執行規則查詢，這是一項昂貴的作業。 不過，一旦在整合的流程資料表中註冊流程，並根據) 符合規則的動作清單 (，所有後續封包都會根據統一的流程資料表專案進行處理。

HNV 原則是由主機代理程式所設計。 每個虛擬機器網路介面卡都設定了 IPv4 位址。 虛擬機器將使用 CA 來彼此通訊，而它們會存在於來自虛擬機器的 IP 封包中。 HNV 會根據存放在主機代理程式資料庫中的網路虛擬化原則，將 CA 框架封裝在 PA 框架中。

![HNV 架構](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)

圖 9：HNV 架構

## <a name="summary"></a>總結
雲端式資料中心可以提供許多優點，例如，改善的延展性和較佳的資源使用率。 若要了解這些潛在優點，需要具備基本上可以在動態環境中處理多組織用戶共享延展性問題的技術。 HNV 的設計就是要解決這些問題，同時透過分離實體網路拓撲的虛擬網路拓撲來提升資料中心的運作效率。 以現有標準為基礎，HNV 會在現今的資料中心執行，並與您現有的 VXLAN 基礎結構搭配運作。 具有 HNV 的客戶現在可以將其資料中心合併到私人雲端，或使用混合式雲端順暢地將其資料中心延伸至主控伺服器提供者的環境。

## <a name="see-also"></a><a name="BKMK_LINKS"></a>另請參閱
若要深入瞭解 HNVv2，請參閱下列連結：


|       內容類型       |                                                                                                                                              參考                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **社群資源**  |                                                                -   [私用雲端架構的 Blog](https://blogs.technet.com/b/privatecloud)<br />-詢問問題：[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **RFC**          |                                                                   -   [NVGRE 草稿 RFC](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN-RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **相關技術** | -如需 Windows Server 2012 R2 中的 Hyper-v 網路虛擬化技術詳細資料，請參閱[Hyper-v 網路虛擬化技術詳細資料](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134174(v=ws.11))<br />-   [網路控制卡](../../../sdn/technologies/network-controller/Network-Controller.md) |