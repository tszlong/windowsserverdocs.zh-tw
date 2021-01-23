---
title: Windows Server 2016 中的 hyper-v 網路虛擬化技術詳細資料
description: 本主題提供有關 Windows Server 2016 中 Hyper-v 網路虛擬化的技術資訊。
manager: grcusanz
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/07/2020
ms.openlocfilehash: 3e7a4c5d52ae9fff29e558bcd8d0bdbc3c9d7d0a
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716964"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Windows Server 2016 中的 hyper-v 網路虛擬化技術詳細資料

>適用於：Windows Server 2019、Windows Server 2016

伺服器虛擬化可以讓多個伺服器執行個體同時在單一實體主機上執行；且伺服器執行個體還可以彼此獨立。 每部虛擬機器的基本操作方式就像是實體電腦上唯一執行的伺服器一樣。

網路虛擬化提供類似的功能，其中多個虛擬網路 (可能有重迭的 IP 位址) 在相同的實體網路基礎結構上執行，而且每個虛擬網路的運作方式就像是在共用網路基礎結構上執行的唯一虛擬網路一樣。 圖 1 顯示此關係。

![伺服器虛擬化與網路虛擬化](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)

圖 1：伺服器虛擬化與網路虛擬化

## <a name="hyper-v-network-virtualization-concepts"></a>Hyper-V 網路虛擬化概念
在 Hyper-v 網路虛擬化 (HNV) 中，客戶或租使用者會定義為部署于企業或資料中心的一組 IP 子網的「擁有者」。 客戶可以是在私人資料中心內具有多個部門或業務單位的公司或企業，需要網路隔離，或是由服務提供者裝載的公用資料中心內的租使用者。 每個客戶都可以在資料中心擁有一或多個 [虛擬網路](#VirtualNetworks) ，且每個虛擬網路都包含一或多個 [虛擬子網](#VirtualSubnets)。

有兩個可在 Windows Server 2016 中使用的 HNV 實施： HNVv1 和 HNVv2。

-   **HNVv1**

    HNVv1 與 Windows Server 2012 R2 和 System Center 2012 R2 Virtual Machine Manager (VMM) 相容。 HNVv1 的設定依賴 WMI 管理和 Windows PowerShell Cmdlet (透過 System Center VMM 提供) 來定義隔離設定和客戶位址 (CA) -虛擬網路到實體位址 (PA) 對應和路由。 Windows Server 2016 中的 HNVv1 尚未新增任何其他功能，也未規劃任何新功能。

    *   設定小組和 HNV V1 與平臺不相容。

    若要使用 HA NVGRE 閘道，使用者必須使用 LBFO 小組或沒有任何團隊。 Or

    o 使用已部署的網路控制站閘道搭配設定的組合參數。


-   **HNVv2**

    HNVv2 中包含了大量的新功能，而這些功能是使用 Azure 虛擬篩選平臺，在 Hyper-v 交換器中使用 Azure 虛擬篩選平臺 (VFP) 轉送擴充功能來執行。 HNVv2 已與 Microsoft Azure Stack 完全整合，其中包含軟體定義網路 (SDN) 堆疊中的新網路控制站。  虛擬網路原則是透過使用 RESTful NorthBound (NB) API 的 Microsoft [網路控制器](../../../sdn/technologies/network-controller/Network-Controller.md) 來定義，並透過多個 SouthBound INTEFACES (SBI) （包括 OVSDB）向主機代理程式進行檢測。 主機代理程式在強制執行之 Hyper-v 交換器的 VFP 延伸模組中的原則。

    > [!IMPORTANT]
    > 本主題著重于 HNVv2。

### <a name="virtual-network"></a><a name="VirtualNetworks"></a>虛擬網路

-   每個虛擬網路都包含一或多個虛擬子網。 虛擬網路會形成隔離界限，其中虛擬網路內的虛擬機器只能彼此通訊。 傳統上，這種隔離是使用具有隔離 IP 位址範圍和 802.1 q 標記或 VLAN 識別碼的 Vlan 來強制執行。 但使用 HNV 時，會使用 NVGRE 或 VXLAN 封裝來強制執行隔離，以建立重迭網路，而且客戶或租使用者之間可能會有重迭的 IP 子網。

-   每個虛擬網路在主機上都有唯一的路由網域識別碼 (RDID) 。 此 RDID 大致對應至資源識別碼，以識別網路控制站中的虛擬網路 REST 資源。 使用統一資源識別項來參考虛擬網路 REST 資源 (URI) 命名空間與附加的資源識別碼。

### <a name="virtual-subnets"></a><a name="VirtualSubnets"></a>虛擬子網路

-   虛擬子網路會針對相同虛擬子網路中的虛擬機器，實作第 3 層 IP 子網路語意。 虛擬子網形成廣播網域 (類似于 VLAN) 並且使用 NVGRE 租使用者網路識別碼 (TNI) 或 VXLAN 網路識別碼 (VNI) 欄位來強制執行隔離。

-   每個虛擬子網都屬於單一虛擬網路 (RDID) ，而且會使用封裝的封包標頭中的 TNI 或 VNI 金鑰，將唯一的虛擬子網識別碼 (VSID) 指派給它。 VSID 在資料中心內必須是唯一的，而且在4096到 2 ^ 24-2 的範圍內。

虛擬網路和路由網域的主要優點是，它可讓客戶攜帶自己的網路拓撲 (例如，將 IP 子網) 至雲端。 圖 2 顯示的範例是 Contoso 公司擁有兩個獨立的網路：R&D 網路和銷售網路。 由於這兩個網路具有不同的路由網域識別碼，因此彼此無法互動。 也就是說，Contoso R&D Net 與 Contoso Sales Net 隔離，即使兩者都是由 Contoso Corp 所擁有也一樣。 Contoso R&D Net 包含三個虛擬子網。 請注意，RDID 和 VSID 在資料中心內都是唯一的。

![客戶網路和虛擬子網路](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)

圖 2：客戶網路和虛擬子網路

**第2層轉送**

在 [圖 2] 中，VSID 5001 中的虛擬機器可以透過 Hyper-v 交換器，將其封包轉送到也在 VSID 5001 中的虛擬機器。 從 VSID 5001 中的虛擬機器傳入的封包會傳送至 Hyper-v 交換器上的特定 VPort。 輸入規則 (例如 encap) 和對應 (例如，將這些封包的 Hyper-v 參數套用至封裝標頭) 。 然後，封包會轉送到 Hyper-v 交換器上的不同 VPort (如果目的地虛擬機器連接至相同的主機) 或不同主機上的不同 Hyper-v 交換器 (如果目的地虛擬機器位於不同的主機) 上。

**第3層路由**

同樣地，VSID 5001 中的虛擬機器可以將它們的封包路由傳送至 VSID 5002 或 VSID 5003 中的虛擬機器，而 HNV 分散式路由器出現在每部 Hyper-v 主機的 VSwitch 中。 傳遞封包至 Hyper-v 交換器時，HNV 會將傳入封包的 VSID 更新為目的地虛擬機器的 VSID。 只有當這兩個 VSID 位於相同 RDID 時才會發生此情況。  因此，具有 RDID1 的虛擬網路介面卡無法將封包傳送至具有 RDID2 的虛擬網路介面卡，而不需要遍歷閘道。

> [!NOTE]
> 在上述的封包流程描述中，「虛擬機器」一詞實際上是指虛擬機器上的虛擬網路介面卡。 常見情況是虛擬機器只會有單一虛擬網路介面卡。 在此情況下，「虛擬機器」和「虛擬網路介面卡」等字的概念可能會是相同的。

每個虛擬子網路都會定義第 3 層 IP 子網路，而第 2 層 (L2) 廣播網域界限會與 VLAN 類似。 當虛擬機器廣播封包時，HNV 會使用單播複寫 (UR) 建立原始封包的複本，並以相同 VSID 中每個 VM 的位址取代目的地 IP 和 MAC。

> [!NOTE]
> 當 Windows Server 2016 發行時，將會使用單播複寫來執行廣播和子網多播。 不支援跨子網多播路由和 IGMP。

除了做為廣播網域之外，VSID 還提供隔離功能。 HNV 中的虛擬網路介面卡連線到 Hyper-v 交換器埠，此埠會將 ACL 規則直接套用至埠 (virtualNetworkInterface REST 資源) 或虛擬子網 (VSID) 。

Hyper-v 交換器埠必須套用 ACL 規則。 此 ACL 可允許全部、全部拒絕或更明確地根據5個元組 (來源 IP、目的地 IP、來源埠、目的地埠、通訊協定) 比對，來允許特定類型的流量。

> [!NOTE]
> Hyper-v 交換器擴充功能將無法在新的軟體定義網路 (SDN) 堆疊中使用 HNVv2。 HNVv2 是使用 Azure 虛擬篩選平臺 (VFP) 交換器擴充功能來執行，其無法與任何其他協力廠商交換器擴充功能一起使用。

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>在 Hyper-v 網路虛擬化中切換和路由
HNVv2 可將正確的第2層 (L2) 切換和第3層 (L3) 路由語義，以作為實體交換器或路由器運作。 當連線到 HNV 虛擬網路的虛擬機器嘗試與相同虛擬子網中的另一部虛擬機器建立連線時 (VSID) 需要先瞭解遠端虛擬機器的 CA MAC 位址。 如果來源虛擬機器的 ARP 表格中有目的地虛擬機器 IP 位址的 ARP 專案，則會使用此專案中的 MAC 位址。 如果專案不存在，來源虛擬機器將會傳送 ARP 廣播，要求提供對應至目的地虛擬機器 IP 位址的 MAC 位址。 Hyper-v 交換器將會攔截此要求，並將它傳送至主機代理程式。 主機代理程式將會在其本機資料庫中尋找所要求目的地虛擬機器 IP 位址的對應 MAC 位址。

> [!NOTE]
> 作為 OVSDB 伺服器的主機代理程式，會使用 VTEP 架構的變體來儲存 CA PA 對應、MAC 資料表等等。

如果有可用的 MAC 位址，主機代理程式就會插入 ARP 回應，並將其傳回給虛擬機器。 虛擬機器的網路堆疊擁有所有必要的 L2 標頭資訊之後，會將框架傳送到 V 交換器上對應的 Hyper-v 埠。 就內部而言，Hyper-v 交換器會針對指派給 V 埠的 N 元組比對規則來測試這個畫面格，並根據這些規則將某些轉換套用至框架。 最重要的是，會根據網路控制站上定義的原則，套用一組封裝轉換，以使用 NVGRE 或 VXLAN 來建立封裝標頭。 根據主機代理程式所程式設計的原則，會使用 CA PA 對應來判斷目的地虛擬機器所在的 Hyper-v 主機 IP 位址。 Hyper-v 交換器可確保將正確的路由規則和 VLAN 標記套用至外部封包，使其到達遠端 PA 位址。

如果連線至 HNV 虛擬網路的虛擬機器想要建立與不同虛擬子網中虛擬機器的連線 (VSID) ，封包必須據以路由傳送。 HNV 假設有一個星狀拓撲，CA 空間中只會有一個 IP 位址可作為下一個躍點，以達到所有 IP 首碼 (意指一個預設路由/閘道) 。 目前，這會強制執行單一預設路由的限制，且不支援非預設路由。

### <a name="routing-between-virtual-subnets"></a>在虛擬子網路間路由
在實體網路中，IP 子網是第2層 (L2) 網域，其中 (虛擬和實體) 的電腦可以直接彼此通訊。 L2 網域是一個廣播網域，其中 ARP 專案 (IP： MAC 位址對應) 透過在所有介面上廣播的 ARP 要求來學習，並將 ARP 回應傳送回要求的主機。 電腦會使用從 ARP 回應中學到的 MAC 資訊，以完整地建造 L2 框架（包括乙太網路標頭）。 但是，如果 IP 位址位於不同的 L3 子網中，ARP 要求就不會跨越此 L3 界限。 相反地，L3 路由器介面 (下一個躍點或預設閘道) 具有來源子網中的 IP 位址，必須使用自己的 MAC 位址來回應這些 ARP 要求。

在標準 Windows 網路中，系統管理員可以建立靜態路由，並將其指派給網路介面。 此外，「預設閘道」通常會設定為介面上的下個躍點 IP 位址，也就是傳送預設路由 (0.0.0.0/0) 的封包。 如果沒有特定的路由存在，封包就會傳送到此預設閘道。 這通常是實體網路的路由器。  HNV 使用內建的路由器，其為每個主機的一部分，且在每個 VSID 中都有介面，可為虛擬網路 (的) 建立分散式路由器。

由於 HNV 採用星狀拓撲，因此 HNV 分散式路由器會作為在相同 VSID 網路中的虛擬子網之間的所有流量的單一預設閘道。 作為預設閘道使用的位址預設為 VSID 中的最低 IP 位址，並指派給 HNV 分散式路由器。 此分散式路由器讓 VSID 網路內的所有流量都能以非常有效率的方式路由傳送，因為每一部主機都可以直接將流量路由傳送至適當的主機，而不需要媒介。  當相同的實體主機上的兩部虛擬機器位於相同 VM 網路，但不同虛擬子網路時，這非常有用。  您將會在本節稍後看到，封包完全不需要離開實體主機。

### <a name="routing-between-pa-subnets"></a>PA 子網之間的路由
相較于為每個虛擬子網 (VSID) 配置一個 PA IP 位址的 HNVv1，HNVv2 現在會針對每個 Switch-Embedded 小組使用一個 PA IP 位址 (設定) NIC 小組成員。 預設部署會假設有兩個 NIC 小組，並為每個主機指派兩個 PA IP 位址。 單一主機具有從相同提供者指派的 PA Ip (PA) 邏輯子網位於相同的 VLAN 上。 相同虛擬子網中的兩個租使用者 Vm 可能確實位於兩部不同的主機，而這些主機連接到兩個不同的提供者邏輯子網。 HNV 會根據 CA PA 對應，為封裝的封包建立外部 IP 標頭。 不過，它依賴主機 TCP/IP 堆疊到預設的 PA 閘道的 ARP，然後根據 ARP 回應建立外部乙太網路標頭。 一般來說，此 ARP 回應來自于主機連接的實體交換器或 L3 路由器上的 SVI 介面。 因此，HNV 會依賴 L3 路由器來路由傳送者邏輯子網/Vlan 之間的封裝封包。

### <a name="routing-outside-a-virtual-network"></a>在虛擬網路外路由
多數的客戶部署將需要從 HNV 環境與不屬於 HNV 環境的資源通訊。 這樣必須要有網路虛擬化閘道，才允許在這兩種環境之間通訊。 需要 HNV 閘道的基礎結構包括私用雲端和混合式雲端。 基本上，在內部和外部 (實體) 網路之間的第3層路由需要 HNV 閘道 (包括 NAT) 或不同網站和/或雲端之間，使用 IPSec VPN 或 GRE 通道 (私用或公用) 。

閘道可以有不同的實際規格。 這些裝置可以建立在 Windows Server 2016 上，整合到機架的最上層 (TOR) 交換器作為 VXLAN 閘道，可透過負載平衡器所通告的虛擬 IP (VIP) 、放入其他現有的網路設備，或可以是新的獨立網路設備。

如需 Windows RAS 閘道選項的詳細資訊，請參閱 [RAS 閘道](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。

## <a name="packet-encapsulation"></a>封包壓縮
HNV 中的每個虛擬網路介面卡都會與下列兩個 IP 位址相關聯：

-   **客戶位址** (CA 根據其內部網路基礎結構) 客戶指派的 IP 位址。 此位址可讓客戶與虛擬機器切換式網路流量，就像尚未移至公用或私人雲端一樣。 虛擬機器可以看見 CA，而客戶也可以連線至該 CA。

-   **提供者位址** (PA) 主機提供者所指派的 IP 位址，或以其實體網路基礎結構為基礎的資料中心系統管理員。 PA 會出現在網路的封包中，這些封包會與執行裝載虛擬機器之 Hyper-V 的伺服器進行交換。 您可以在實體網路上看見 PA，但虛擬機器看不見該 PA。

CA 會保留客戶的網路拓樸，這樣會虛擬化並分隔實際基礎實體網路拓樸和位址，如同 PA 所實作。 下圖顯示網路虛擬化之後的虛擬機器 CA 和網路基礎結構 PA 之間的概念性關係。

![實體基礎結構網路虛擬化的概念圖](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)

圖 6：實體基礎結構的網路虛擬化概念性圖表

在此圖中，客戶虛擬機器正在傳送 CA 空間中的資料封包，以透過自己的虛擬網路或「通道」來跨越實體網路基礎結構。 在上述範例中，您可以將通道視為 Contoso 和 Fabrikam 資料封包的「信封」，並以綠色出貨標籤 (PA 位址) 從左邊的來源主機傳遞給右邊的目的地主機。 機碼是主機如何判斷「送貨位址」 (PA 的) 對應至 Contoso 和 Fabrikam CA、「信封」如何放在封包中，以及目的地主機如何解除包裝封包，並正確傳遞給 Contoso 和 Fabrikam 目的地虛擬機器。

這個簡單的比喻可以突顯網路虛擬化的重點概念：

-   每個虛擬機器 CA 都會對應到實體主機 PA。 而多個 CA 可以和同一 PA 建立關聯。

-   虛擬機器會在 CA 空間中傳送資料封包，這些封包會根據對應放入具有 PA 來源和目的地配對的「信封」。

-   CA-PA 對應必須能夠讓主機區分不同客戶虛擬機器的封包。

如此一來，將網路虛擬化的機制就是將虛擬機器使用的網路位址虛擬化。 網路控制站負責位址對應，而主機代理程式會使用 MS_VTEP 架構來維護對應資料庫。 下一節將說明實際的位址虛擬化機制。

## <a name="network-virtualization-through-address-virtualization"></a>透過位址虛擬化進行網路虛擬化
HNV 會使用網路虛擬化的一般路由封裝 (NVGRE) 或虛擬可延伸的區域網路 (VXLAN) ，來實行重迭租使用者網路。  預設值為 VXLAN。

### <a name="virtual-extensible-local-area-network-vxlan"></a>虛擬可擴充區域網路 (VXLAN) 
虛擬可延伸的區域網路 (VXLAN)  ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) 通訊協定已廣泛採用，並支援來自 Cisco、織錦、Arista、DELL、HP 和其他廠商的廠商。 VXLAN 通訊協定使用 UDP 作為傳輸。 適用于 VXLAN 的 IANA 指派 UDP 目的地埠是4789，而 UDP 來源埠應為內部封包中要用於 ECMP 分配的資訊雜湊。 在 UDP 標頭之後，VXLAN 標頭會附加至封包，其中包含保留的4位元組欄位，後面接著 VXLAN 網路識別碼的3個位元組欄位 (VNI) -VSID，後面接著另一個保留的1位元組欄位。 在 VXLAN 標頭之後，將會附加原始 CA L2 框架 (而不會附加 CA Ethernet 框架的) 。

![VXLAN 包頭](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)

### <a name="generic-routing-encapsulation-nvgre"></a> (NVGRE) 的一般路由封裝
此網路虛擬化機制使用一般路由封裝 (NVGRE) 作為通道標頭的一部分。 在 NVGRE 中，虛擬機器的封包會封裝在另一個封包中。 這個新封包的標頭除了虛擬子網路識別碼之外，還擁有適當的來源與目的地 PA IP 位址，虛擬子網路識別碼儲存於 GRE 標頭的索引鍵欄位中，如圖 7 所示。

![NVGRE 封裝](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)

圖 7：網路虛擬化 - NVGRE 封裝

虛擬子網識別碼可讓主機針對任何指定的封包找出客戶虛擬機器，即使 PA 的和 CA 的封包可能重迭也一樣。 這樣可讓相同主機上的所有虛擬機器共用單一 PA，如圖 7 所示。

共用 PA 對網路延展性有很大的影響。 網路基礎結構需要知道的 IP 和 MAC 位址數目會大幅減少。 例如，如果每部終端主機平均擁有 30 部虛擬機器，則網路基礎結構需要知道的 IP 和 MAC 位址數目會降低 30 倍。封包中內嵌的虛擬子網路識別碼也能夠輕易地將封包相互關聯到實際的客戶。

Windows Server 2012 R2 的 PA 共用配置是每部主機每個 VSID 的一個 PA。 若為 Windows Server 2016，配置為每個 NIC 小組成員一個 PA。

在 Windows Server 2016 和更新版本中，HNV 完全支援現成的 NVGRE 和 VXLAN;不需要升級或購買新的網路硬體，例如 Nic (網路介面卡) 、交換器或路由器。 這是因為網路上的這些封包是 PA 空間中的一般 IP 封包，與現今的網路基礎結構相容。  不過，若要獲得最佳效能，請使用支援工作卸載的最新驅動程式所支援的 Nic。

## <a name="multi-tenant-deployment-example"></a>多租用戶部署範例
下圖顯示在雲端資料中心內，兩個客戶的部署範例，其中包含網路原則所定義的 CA-PA 關聯性。

![多租用戶部署範例](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)

圖 8：多組織用戶共享部署範例

討論圖 8 中的範例。 移到裝載提供者的共用 IaaS 服務之前：

-   Contoso Corp 會在 IP 位址 10.1.1.11 上執行 SQL Server (名為 **SQL**)，在 IP 位址 10.1.1.12 上執行網頁伺服器 (名為 **Web**) 並使用 SQL Server 進行資料庫交易。

-   Fabrikam Corp 會執行 SQL Server (也稱為 **SQL**) 並指派 IP 位址 10.1.1.11，以及執行網頁伺服器 (也稱為 **Web**)，IP 位址為 10.1.1.12 並使用 SQL Server 進行資料庫交易。

我們會假設主機服務提供者先前已透過網路控制站建立提供者 (PA) 邏輯網路，以對應至其實體網路拓撲。 網路控制站會從連接主機的邏輯子網 IP 首碼配置兩個 PA IP 位址。 網路控制站也會指出適當的 VLAN 標記以套用 IP 位址。

使用網路控制站、Contoso Corp 和 Fabrikam Corp，然後建立其虛擬網路和子網（由提供者支援） (PA) 由主機服務提供者指定的邏輯網路。 Contoso Corp 和 Fabrikam Corp 都各自將 SQL Server 和網頁伺服器移到相同裝載提供者的共用 IaaS 服務，且剛好都在 Hyper-V 主機 1 上執行 **SQL** 虛擬機器，以及在 Hyper-V 主機 2 上執行 **Web** (IIS7) 虛擬機器。 所有虛擬機器都會保留原始的內部網路 IP 位址 (它們的 CA)。

這兩個公司都會將下列虛擬子網識別碼指派給網路控制站)  (VSID，如下所示。  每一部 Hyper-v 主機上的主機代理程式都會從網路控制站接收配置的 PA IP 位址，並在非預設的網路區間內建立兩個 PA 主機 Vnic。 網路介面會指派給每個主機 Vnic，其中指派了 PA IP 位址，如下所示：

-   Contoso Corp 的虛擬機器 VSID 和 PAs： **VSID** 是5001， **SQL PA** 為192.168.1.10， **Web pa** 為192.168.2.20

-   Fabrikam Corp 的虛擬機器 VSID 和 PAs： **VSID** 為6001， **SQL PA** 為192.168.1.10， **Web pa** 為192.168.2.20

網路控制站會連接所有網路原則 (包括) 到 SDN 主機代理程式的 CA-PA 對應，這會在 OVSDB 資料庫資料表) 的持續性存放區 (中維護原則。

當 Hyper-v 主機2上 (10.1.1.12) 的 Contoso Corp Web 虛擬機器在10.1.1.11 建立與 SQL Server 的 TCP 連線時，會發生下列情況：

-   10.1.1.11 目的地 MAC 位址的 VM Arp

-   VSwitch 中的 VFP 延伸模組會攔截此封包，並將其傳送到 SDN 主機代理程式

-   SDN 主機代理程式會在其10.1.1.11 的 MAC 位址中尋找其原則存放區

-   如果找到 MAC，主機代理程式就會將 ARP 回應插入至 VM

-   如果找不到 MAC，則不會傳送任何回應，而且 VM 中10.1.1.11 的 ARP 專案會標示為無法連線。

-   VM 現在會使用正確的 CA Ethernet 和 IP 標頭來建立 TCP 封包，並將其傳送至 vSwitch

-   VSwitch 中的 VFP 轉送延伸模組會透過 VFP 層處理此封包 (如下所述) 指派給接收封包的來源 vSwitch 埠，並在 VFP 統一的流程資料表中建立新的流程輸入

-   VFP 引擎會根據 IP 和乙太網路標頭，執行每個圖層的規則比對或流程資料表查閱 (例如虛擬網路層) 。

-   虛擬網路層中的相符規則會參考 CA PA 對應空間並執行封裝。

-   封裝類型 (VXLAN 或 NVGRE) 是在 VNet 層中指定，並與 VSID 一併指定。

-   在 VXLAN 封裝的情況下，會使用 VXLAN 標頭中的 VSID 5001 來建立外部 UDP 標頭。
    外部 IP 標頭會根據 SDN 主機代理程式的原則存放區，以指派給 Hyper-v 主機 2 (192.168.2.20) 和 Hyper-v 主機 1 (192.168.1.10) 的來源和目的地 PA 位址來建立。

-   然後，此封包會流向 VFP 中的 PA 路由層。

-   VFP 中的 PA 路由圖層會參考用於 PA 空間流量的網路區間和 VLAN 識別碼，並使用主機的 TCP/IP 堆疊，正確地將 PA 封包轉送至 Hyper-v 主機1。

-   在收到封裝的封包時，Hyper-v 主機1會接收 PA 網路區間中的封包，並將其轉送到 vSwitch。

-   VFP 會透過其 VFP 層處理封包，並在 VFP 統一的流程資料表中建立新的流程專案。

-   VFP 引擎會比對虛擬網路層中的 ingres 規則，並去除外部封裝的乙太網路、IP 和 VXLAN 標頭。

-   VFP 引擎接著會將封包轉送到目的地 VM 所連接的 vSwitch 埠。

Fabrikam Corp **Web** 和 **SQL** 虛擬機器會使用 Fabrikam Corp 的 HNV 原則設定執行類似的流量處理程序。如此一來，HNV、Fabrikam Corp 與 Contoso Corp 虛擬機器就可以進行互動，如同在原始內部網路一樣。 即使它們使用相同的 IP 位址，也絕對無法彼此互動。

不同的位址 (CAs 和 PAs) 、Hyper-v 主機的原則設定，以及 CA 與輸入和輸出虛擬機器流量的 PA 之間的位址轉換，會使用 NVGRE 金鑰或 VLXAN VNID 來隔離這些伺服器集合。 此外，虛擬化對應和轉換會分隔實體網路基礎架構與虛擬網路架構。 即使 Contoso **SQL** 和 **Web** 及 Fabrikam **SQL** 和 **Web** 都位於它們自己的 CA IP 子網路 (10.1.1/24) 內，其實體部署都發生在不同 PA 子網路 192.168.1/24 和 192.168.2/24 各自的主機上。 這暗示跨子網路的虛擬機器佈建和即時移轉可以透過 HNV 執行。

## <a name="hyper-v-network-virtualization-architecture"></a>Hyper-V 網路虛擬化架構
在 Windows Server 2016 中，HNVv2 是使用 Azure 虛擬篩選平臺 (VFP) ，也就是 Hyper-v 交換器內的 NDIS 篩選擴充功能。 VFP 的重要概念是，Match-Action 流程引擎，並將內部 API 公開給 SDN 主機代理程式，以進行網路原則的程式設計。 SDN 主機代理程式本身會透過 OVSDB 和 WCF SouthBound 通道，從網路控制站接收網路原則。 虛擬網路原則 (例如 CA-PA 對應) 使用 VFP 進行程式設計，但使用 Acl、QoS 等其他原則。

VSwitch 和 VFP 轉送擴充功能的物件階層如下所示：

-   vSwitch

    -   外部 NIC 管理

    -   NIC 硬體卸載

    -   全域轉送規則

    -   Port

        -   用來釘選的輸出轉送層

        -   對應和 NAT 集區的空間清單

        -   整合的流程資料表

        -   VFP 圖層

            -   Flow 資料表

            -   Group

            -   規則

                -   規則可以參考空間

在 VFP 中，每一種原則類型都會建立一個層 (例如，虛擬網路) ，而這是一組一般的規則/流動資料表。 在將特定規則指派給該層以執行這類功能之前，它沒有任何內建功能。 系統會為每個圖層指派優先權，並依遞增優先順序將層指派給埠。 根據方向和 IP 位址系列，規則會組織成群組。 群組也會被指派優先順序，而且最多隻能有一個群組的規則符合指定的流程。

具有 VFP 擴充功能的 vSwitch 轉送邏輯如下所示：

-   輸入處理 (從傳入埠的封包觀點來輸入) 

-   轉發

-   輸出處理 (從離開埠的封包觀點來看輸出) 

VFP 支援 NVGRE 和 VXLAN 封裝類型的內部 MAC 轉送，以及外部 MAC VLAN 型轉送。

VFP 延伸模組具有慢速路徑和快速路徑，可進行封包遍歷。 流程中的第一個封包必須跨越每個圖層中的所有規則群組，並進行一項需要昂貴作業的規則查閱。 不過，一旦流程註冊在整合的流程資料表中，並根據符合的規則來 (動作清單時) 所有後續的封包都會根據統一的流程資料表專案來處理。

HNV 原則由主機代理程式進行程式設計。 每個虛擬機器網路介面卡都會設定 IPv4 位址。 虛擬機器將使用 CA 來彼此通訊，而它們會存在於來自虛擬機器的 IP 封包中。 HNV 會根據儲存在主機代理程式資料庫中的網路虛擬化原則，將 CA 框架封裝在 PA 框架中。

![HNV 架構](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)

圖 9：HNV 架構

## <a name="summary"></a>[摘要]
雲端式資料中心可以提供許多優點，例如，改善的延展性和較佳的資源使用率。 若要了解這些潛在優點，需要具備基本上可以在動態環境中處理多組織用戶共享延展性問題的技術。 HNV 的設計就是要解決這些問題，同時透過分離實體網路拓撲的虛擬網路拓撲來提升資料中心的運作效率。 根據現有的標準，HNV 會在現今的資料中心內執行，並與您現有的 VXLAN 基礎結構一起運作。 具有 HNV 的客戶現在可以將其資料中心合併至私用雲端，或將其資料中心順暢地延伸至具有混合式雲端的主機伺服器提供者環境。

## <a name="see-also"></a><a name="BKMK_LINKS"></a>另請參閱
若要深入瞭解 HNVv2，請參閱下列連結：


|       內容類型       |                                                                                                                                              參考                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **社群資源**  |                                                                -   [私用雲端架構的 Blog](https://blogs.technet.com/b/privatecloud)<br />-詢問問題： [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **RFC**          |                                                                   -   [NVGRE 草稿 RFC](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN-RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **相關技術** | -如需 Windows Server 2012 R2 中的 Hyper-v 網路虛擬化技術詳細資料，請參閱 [Hyper-v 網路虛擬化技術詳細資料](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134174(v=ws.11))<br />-   [網路控制站](../../../sdn/technologies/network-controller/Network-Controller.md) |