---
title: HYPER-V 網路虛擬化技術詳細資料在 Windows Server 2016
description: 本主題提供有關 Windows Server 2016 中的 HYPER-V 網路虛擬化技術的資訊
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b33c96ff7150b0dc4f6f93d2fa24741c9762a799
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282289"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>HYPER-V 網路虛擬化技術詳細資料在 Windows Server 2016

>適用於：Windows Server 2016

伺服器虛擬化可以讓多個伺服器執行個體同時在單一實體主機上執行；且伺服器執行個體還可以彼此獨立。 每部虛擬機器的基本操作方式就像是實體電腦上唯一執行的伺服器一樣。  

網路虛擬化提供類似的功能、 （可能具備重疊 IP 位址） 的多個虛擬網路執行同一個實體網路基礎結構和每個虛擬網路運作方式就好像它是唯一的虛擬網路上執行共用的網路基礎結構。 圖 1 顯示此關係。  

![伺服器虛擬化與網路虛擬化](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  

圖 1：伺服器虛擬化與網路虛擬化  

## <a name="hyper-v-network-virtualization-concepts"></a>Hyper-V 網路虛擬化概念  
在 HYPER-V 網路虛擬化 (HNV) 中，客戶或租用戶定義為一組會部署在 enterprise 或 datacenter 的 IP 子網路的 「 擁有者 」。 客戶可以是公司或企業版含多個部門或私人資料中心內的業務單位需要網路隔離或服務提供者所裝載的公用資料中心中的租用戶。 每個客戶可以有一或多個[虛擬網路](#VirtualNetworks)中的資料中心，以及每個虛擬網路都包含一或多個[虛擬子網路](#VirtualSubnets)。  

有兩個 HNV 實作將會提供 Windows Server 2016 中：HNVv1 和 HNVv2。  

-   **HNVv1**  

    HNVv1 適用於 Windows Server 2012 R2 和 System Center 2012 R2 Virtual Machine Manager (VMM)。 設定 HNVv1 依賴 WMI 管理和 （引導式透過 System Center VMM） 的 Windows PowerShell cmdlet 來定義的隔離設定和客戶位址 (CA)-虛擬網路-實體位址 (PA) 對應和路由。 HNVv1 Windows Server 2016 中已新增任何額外的功能和任何新功能已計劃。  

    • 可讓您設定小組和 HNV V1 不相容的平台。

    o 以使用 HA NVGRE 閘道使用者需要為使用 LBFO 小組或沒有小組。 或

    o 設定使用網路控制站部署閘道組合交換器。


-   **HNVv2**  

    大量的新功能包括在 HNVv2 實作使用 Azure 虛擬篩選平台 (VFP) 轉送在 HYPER-V 交換器的擴充功能。 HNVv2 已完全整合與 Microsoft Azure Stack 軟體定義網路 (SDN) 堆疊中包含新的網路控制站。  虛擬網路原則透過 Microsoft 定義[網路控制卡](../../../sdn/technologies/network-controller/Network-Controller.md)使用 RESTful NorthBound) (NB) API，並連接至主機代理程式透過多個 SouthBound Intefaces (SBI) 包括 OVSDB。 VFP 擴充功能，其中就會強制執行的 HYPER-V 交換器的主機代理程式原則。  

    > [!IMPORTANT]  
    > 本主題著重於 HNVv2。  

### <a name="VirtualNetworks"></a>虛擬網路  

-   每個虛擬網路都包含一或多個虛擬子網路。 虛擬網路會形成隔離界限內的虛擬網路的虛擬機器位置只能彼此通訊。 傳統上，這種隔離已強制使用隔離的 IP 位址範圍和 802.1q Vlan 標記或 VLAN id。 但是，透過 HNV，隔離會強制執行使用 NVGRE 或 VXLAN 封裝可能會重疊的 IP 子網路之間的客戶或租用戶中建立覆疊網路。  

-   每個虛擬網路在主機上具有唯一的路由網域識別碼 (RDID)。 此 RDID 大致對應來識別虛擬網路中網路控制站的 REST 資源的資源識別碼。 虛擬網路 REST 資源參考使用統一資源識別元 (URI) 的命名空間，並提供附加的資源識別碼。  

### <a name="VirtualSubnets"></a>虛擬子網路  

-   虛擬子網路會針對相同虛擬子網路中的虛擬機器，實作第 3 層 IP 子網路語意。 虛擬子網路會形成一個廣播的網域 （類似 VLAN），並使用 NVGRE 租用戶網路識別碼 (TNI) 或 VXLAN 網路識別碼 (VNI) 欄位會強制執行隔離。  

-   每個虛擬子網路都屬於單一虛擬網路 (RDID)，並為其指派唯一虛擬子網路識別碼 (VSID) 封裝的封包標頭中使用的 TNI 」 或 「 VNI 的索引鍵。 VSID 必須是唯一的資料中心內，而且在範圍內 4096 到 2 ^24-2。  

虛擬網路和路由網域的主要優點是它可讓客戶將自己的網路拓撲 （例如，IP 子網路） 至雲端。 圖 2 顯示的範例是 Contoso 公司擁有兩個獨立的網路：R&D 網路和銷售網路。 由於這兩個網路具有不同的路由網域識別碼，因此彼此無法互動。 也就是說，Contoso R&D 網路與 Contoso 銷售網路相互隔離，即使這兩者的擁有者都是 Contoso 公司。Contoso R&D Net 包含三個虛擬子網路。 請注意，RDID 和 VSID 在資料中心內都是唯一的。  

![客戶網路和虛擬子網路](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  

圖 2：客戶網路和虛擬子網路  

**第 2 層轉送**  

[圖 2] 中 VSID 5001 的虛擬機器可以有封包轉送到也是透過 HYPER-V 交換器的 VSID 5001 的虛擬機器。 從 VSID 5001 的虛擬機器的連入封包傳送到特定的 VPort HYPER-V 交換器上。 輸入規則 （例如，壓縮） 和對應 （例如封裝標頭） 會套用 HYPER-V 交換器，這些封包。 封包然後轉送到 HYPER-V 交換器上的不同 VPort （如果目的地虛擬機器已連結到相同的主機） 或不同的 HYPER-V 交換器，在不同的主控件 （如果目的地虛擬機器位於不同的主機上）。  

**圖層的 3 個路由**  

同樣地，VSID 5001 中的虛擬機器可以有封包路由傳送至 VSID 5002 或 VSID 5003 的虛擬機器，也就是出現在每個 HYPER-V 主機的 VSwitch HNV 分散式路由器。 在將封包遞送到 HYPER-V 交換器，HNV 連入封包的 VSID 更新為目的地虛擬機器的 VSID。 只有當這兩個 VSID 位於相同 RDID 時才會發生此情況。  因此，與 RDID1 的虛擬網路介面卡無法傳送封包到含有 RDID2 的虛擬網路介面卡而不會周遊閘道。  

> [!NOTE]  
> 在上述的封包流程說明，「 虛擬機器 」 實際上是指虛擬機器上的虛擬網路介面卡。 常見情況是虛擬機器只會有單一虛擬網路介面卡。 在此情況下，文字 「 虛擬機器 」 和 「 虛擬網路介面卡 」 可以在概念上的意義相同。  

每個虛擬子網路都會定義第 3 層 IP 子網路，而第 2 層 (L2) 廣播網域界限會與 VLAN 類似。 當虛擬機器會廣播封包時，HNV 會使用單點傳播複寫 (UR) 來建立一份原始封包，並取代每個 VM 位於相同 VSID 的位址上的目的地 IP 和 MAC。  

> [!NOTE]  
> 當 Windows Server 2016 隨附時，會使用單點傳播複寫實作廣播和子網路的多點傳送。 不支援跨子網路多點傳送路由和 IGMP。  

除了做為廣播網域之外，VSID 還提供隔離功能。 HNV 中的虛擬網路介面卡會連接到 HYPER-V 交換器連接埠會套用至連接埠 （virtualNetworkInterface REST 資源） 的直接或它是在其中一部分的虛擬子網路 (VSID) 的 ACL 規則。  

HYPER-V 交換器連接埠必須套用 ACL 規則。 此 ACL 可以允許 ALL，全部拒絕 或更明確，只允許特定類型的流量根據 5 個 tuple （來源 IP、 目的地 IP、 來源連接埠、 目的地連接埠、 通訊協定） 比對。  

> [!NOTE]  
> HYPER-V 交換器擴充功能將無法搭配 HNVv2 中新的軟體定義網路 (SDN) 堆疊。 使用 Azure 虛擬篩選平台 (VFP) 交換器擴充功能無法與任何其他第 3 方交換器擴充功能搭配使用的實作 HNVv2。  

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>切換與 HYPER-V 網路虛擬化路由  
HNVv2 實作正確的第 2 層 (L2) 切換和第 3 層 (L3) 路由語意運作就如同實體交換器或路由器會運作。 當虛擬機器連線到 HNV 虛擬網路會嘗試位於相同虛擬子網路 (VSID) 若要了解遠端虛擬機器的 CA MAC 位址必須先建立與另一部虛擬機器的連線。 如果沒有在來源虛擬機器的 ARP 表格中的目的地虛擬機器的 IP 位址的 ARP 項目，則會使用此項目從 MAC 位址。 如果項目不存在，會將傳送 ARP 廣播對應到目的地虛擬機器的 IP 位址的 MAC 位址的要求會傳回與來源虛擬機器。 HYPER-V 交換器會攔截此要求，並將它傳送至主機代理程式。 主機代理程式會尋找在其本機資料庫來進行要求的目的地虛擬機器的 IP 位址相對應的 MAC 位址。  

> [!NOTE]  
> 主機代理程式，做為 OVSDB 伺服器中，會使用 VTEP 結構描述的變體來儲存 CA-PA 對應、 MAC 資料表等等。  

如果 MAC 位址可用，主機代理程式會將插入的 ARP 回應，並將此傳回給虛擬機器。 虛擬機器的網路堆疊具有所有必要的 L2 標頭資訊之後，框架會傳送至對應 V 交換器上的 HYPER-V 通訊埠。 就內部而言，HYPER-V 交換器測試這個框架，防止 n-tuple 比對規則指派給 V 連接埠，並將特定轉換套用到以這些規則為基礎的框架。 最重要的是，一組封裝轉換會套用至建構封裝標頭使用 NVGRE 或 VXLAN，根據在網路控制站上定義的原則。 根據主機代理程式的程式設計原則，CA-PA 對應用來判斷 HYPER-V 主機的 IP 位址為目的地虛擬機器所在的位置。 HYPER-V 交換器可確保正確的路由規則，並使它抵達遠端的 PA 位址，將會套用至外部的封包的 VLAN 標籤。  

如果連線到 HNV 虛擬網路的虛擬機器要使用不同的虛擬子網路 (VSID) 中的虛擬機器建立連線時，封包必須據此路由。 HNV 會假設星號拓撲用做為下一個躍點，來連線到所有 IP 前置詞 （意義一個預設路由/閘道） 的 CA 空間中只有一個 IP 位址所在。 目前，這會強制執行單一的預設路由的限制，並不支援非預設路由。  

### <a name="routing-between-virtual-subnets"></a>在虛擬子網路間路由  
在實體網路中，IP 子網路會是第 2 層 (L2) 網域，其中電腦 （虛擬和實體） 可以直接彼此通訊。 L2 網域是一個廣播的網域，其中透過 ARP 要求在所有介面上廣播，學習 ARP 項目 (IP:MAC 位址 map) 和 ARP 回應會傳送回要求的主機。 電腦會使用從 ARP 回應中學到的 MAC 資訊完全建構包括乙太網路標頭的 L2 框架。 不過，如果 IP 位址，不同的 L3 子網路中的 ARP 要求不會跨越此 L3 界限。 相反地，L3 路由器介面 （下個躍點 」 或 「 預設閘道） 來源子網路中的 IP 位址必須回應這些 ARP 要求具有自己的 MAC 位址。  

在標準 Windows 網路中，系統管理員可以建立靜態路由，並將其指派給網路介面。 此外，「 預設閘道 」 通常會設定為目的地的預設路由 (0.0.0.0/0) 的封包傳送至何處介面上的下個躍點 IP 位址。 如果不有任何特定的路由封包傳送到此預設閘道。 這通常是實體網路的路由器。  HNV 會使用內建的路由器，是每個主機的一部分，而且在每個 VSID，若要建立虛擬網路的分散式的路由器介面。  

因為 HNV 會假設星號的拓樸，HNV 分散式的路由器做為單一的預設閘道會屬於相同的 VSID 網路的虛擬子網路之間的所有流量。 做為預設閘道的預設值中的 VSID 的最低 IP 位址，並指派給 HNV 分散式路由器。 此分散式的路由器可讓 VSID 網路中的所有流量正確路由，因為每個主控件可以直接將流量路由到適當的主機不需要媒介非常有效率的方式。  當相同的實體主機上的兩部虛擬機器位於相同 VM 網路，但不同虛擬子網路時，這非常有用。  您將會在本節稍後看到，封包完全不需要離開實體主機。  

### <a name="routing-between-pa-subnets"></a>PA 子網路之間的路由  
相較於 HNVv1 配置一個 PA IP 位址的每個虛擬子網路 (VSID)，HNVv2 現在會使用每個 Switch-Embedded Teaming (SET) NIC 小組成員的一個 PA IP 位址。 預設的部署會假設兩個 NIC 小組，並指派每個主機的兩個 PA IP 位址。 單一主機已從相同提供者 (PA) 邏輯的子網路在相同 VLAN 指派的 PA Ip。 兩個租用戶 Vm 位於相同的虛擬子網路可能確實會放在兩個不同的主機連線到兩個不同的提供者的邏輯子網路上。 HNV 會建構封裝的封包的 CA-PA 對應為基礎的外部 IP 標頭。 不過，它會依賴預設 PA 閘道給 ARP 的主機 TCP/IP 堆疊，並建立根據 ARP 回應外部的乙太網路標頭。 一般而言，此 ARP 回應是來自 SVI 介面上的實體交換器或路由器 L3 主機連線的位置。 因此，HNV 會相依於 L3 路由器的路由封裝提供者的邏輯子網路之間的封包 / Vlan。  

### <a name="routing-outside-a-virtual-network"></a>在虛擬網路外路由  
多數的客戶部署將需要從 HNV 環境與不屬於 HNV 環境的資源通訊。 這樣必須要有網路虛擬化閘道，才允許在這兩種環境之間通訊。 需要 HNV 閘道的基礎結構都包含私用雲端和混合式雲端。 基本上，HNV 閘道所需的第 3 層 （包括 NAT） 的內部和外部 （實體） 的網路或不同站台及/或雲端 （私人或公用） 使用的 IPSec VPN 或 GRE 通道之間的路由。  

閘道可以有不同的實際規格。 閘道可以建置於 Windows Server 2016 中，併入機櫃頂端 (TOR) 交換器作為 VXLAN 閘道，存取透過虛擬 IP (VIP) 通告的負載平衡器放置於其他現有的網路裝置，或可以是新的獨立網路裝置.  

如需有關 Windows RAS 閘道選項的詳細資訊，請參閱[RAS 閘道](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。  

## <a name="packet-encapsulation"></a>封包壓縮  
HNV 中的每個虛擬網路介面卡都會與下列兩個 IP 位址相關聯：  

-   **客戶地址**由客戶根據他們的內部網路基礎結構指派的 (CA) 的 IP 位址。 此位址可讓客戶交換與虛擬機器的網路流量，如同它有未移至公用或私人雲端。 虛擬機器可以看見 CA，而客戶也可以連線至該 CA。  

-   **提供者位址**(PA) 的 IP 位址指派的主機服務提供者或資料中心系統管理員根據他們的實體網路基礎結構。 PA 會出現在網路的封包中，這些封包會與執行裝載虛擬機器之 Hyper-V 的伺服器進行交換。 您可以在實體網路上看見 PA，但虛擬機器看不見該 PA。  

CA 會保留客戶的網路拓樸，這樣會虛擬化並分隔實際基礎實體網路拓樸和位址，如同 PA 所實作。 下圖顯示網路虛擬化之後的虛擬機器 CA 和網路基礎結構 PA 之間的概念性關係。  

![實體基礎結構網路虛擬化的概念圖](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  

圖 6：實體基礎結構網路虛擬化的概念圖  

在圖表中，客戶虛擬機器會傳送資料封包在 CA 空間中，來周遊實體網路基礎結構，透過自己的虛擬網路或 「 通道 」。 在上述範例中，通道可以視為 「 信封 」 的 Contoso 和 Fabrikam 資料封包傳遞從來源主控件，在左邊，右邊的目的地主機綠色的出貨標籤 （PA 位址）。 關鍵在於主機如何判斷 「 出貨地址 」 (PA) 對應到 Contoso 和 Fabrikam CA 的、 如何在 「 信封 」 放入封包，並可以如何開封包的目的地主機並將其傳送至 Contoso 和 Fabrikam目的地虛擬機器正確。  

這個簡單的比喻可以突顯網路虛擬化的重點概念：  

-   每個虛擬機器 CA 都會對應到實體主機 PA。 而多個 CA 可以和同一 PA 建立關聯。  

-   虛擬機器會傳送資料封包，在 CA 空間，這會使 「 信封 」 與 PA 來源與目的地配對對應。  

-   CA-PA 對應必須能夠讓主機區分不同客戶虛擬機器的封包。  

如此一來，將網路虛擬化的機制就是將虛擬機器使用的網路位址虛擬化。 網路控制站會負責位址對應，以及主機代理程式會維護使用 MS_VTEP 結構描述的對應資料庫。 下一節將說明實際的位址虛擬化機制。  

## <a name="network-virtualization-through-address-virtualization"></a>透過位址虛擬化進行網路虛擬化  
HNV 會實作覆疊租用戶網路使用網路虛擬化 Generic Routing Encapsulation (NVGRE) 或虛擬可延伸區域網路 (VXLAN)。  預設值為 VXLAN。  

### <a name="virtual-extensible-local-area-network-vxlan"></a>虛擬可延伸區域網路 (VXLAN)  
虛擬可延伸區域網路 (VXLAN) ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) 通訊協定已被廣泛採用在市場中，例如 Cisco、 Brocade、 Arista、 Dell、 HP 和其他廠商的支援。 VXLAN 通訊協定使用 UDP 作為傳輸。 VXLAN IANA 指派 UDP 目的地連接埠是 4789 及 UDP 來源連接埠應該是雜湊從內部的封包用於 ECMP 分散的資訊。 UDP 標頭之後 VXLAN 標頭會附加至封包包含保留的 4 位元組欄位，後面接著的 3 位元欄位的 VXLAN 網路識別碼 (VNI)-VSID 為後面接著另一個保留 1 個位元組的欄位。 VXLAN 標頭之後會附加原始 CA L2 框架 （不含 CA 乙太網路框架的 FC 中）。  

![VXLAN 封包標頭](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  

### <a name="generic-routing-encapsulation-nvgre"></a>一般路由封裝 (NVGRE)  
這個網路虛擬化機制使用 Generic Routing Encapsulation (NVGRE) 做為通道標頭的一部分。 在 NVGRE 中，虛擬機器的封包封裝在另一個封包。 這個新封包的標頭除了虛擬子網路識別碼之外，還擁有適當的來源與目的地 PA IP 位址，虛擬子網路識別碼儲存於 GRE 標頭的索引鍵欄位中，如圖 7 所示。  

![NVGRE 封裝](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  

圖 7：網路虛擬化 - NVGRE 封裝  

虛擬子網路識別碼可讓主機以識別任何指定封包的客戶虛擬機器，即使的 PA 和 CA 的封包可能會重疊。 這樣可讓相同主機上的所有虛擬機器共用單一 PA，如圖 7 所示。  

共用 PA 對網路延展性有很大的影響。 網路基礎結構需要知道的 IP 和 MAC 位址數目會大幅減少。 例如，如果每部終端主機平均擁有 30 部虛擬機器，則網路基礎結構需要知道的 IP 和 MAC 位址數目會降低 30 倍。封包中內嵌的虛擬子網路識別碼也能夠輕易地將封包相互關聯到實際的客戶。  

適用於 Windows Server 2012 R2 共用配置的 PA 是每個 VSID 的一個 PA 每一部主機。 適用於 Windows Server 2016 此配置會為每個 NIC 小組成員的一個 PA。  

Windows Server 2016 和更新版本，HNV 可以立即支援 NVGRE 和 VXLAN 現成的;它不需要升級或購買新的網路硬體，例如 Nic （網路介面卡）、 交換器或路由器。 這是因為在網路上的這些封包是 PA 空間，這是相容於現今的網路基礎結構中的一般 IP 封包。  不過，若要取得最佳的效能使用 Nic 支援的最新的驅動程式的支援工作卸載。  

## <a name="multi-tenant-deployment-example"></a>多租用戶部署範例  
下圖顯示位於雲端資料中心網路原則所定義的 CA-PA 關係的兩個客戶的部署範例。  

![多租用戶部署範例](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  

圖 8：多租用戶部署範例  

討論圖 8 中的範例。 移到裝載提供者的共用 IaaS 服務之前：  

-   Contoso Corp 會在 IP 位址 10.1.1.11 上執行 SQL Server (名為 **SQL**)，在 IP 位址 10.1.1.12 上執行網頁伺服器 (名為 **Web**) 並使用 SQL Server 進行資料庫交易。  

-   Fabrikam Corp 會執行 SQL Server (也稱為 **SQL** ) 並指派 IP 位址 10.1.1.11，以及執行網頁伺服器 (也稱為 **Web** )，IP 位址為 10.1.1.12 並使用 SQL Server 進行資料庫交易。  

我們將假設主機服務提供者具有先前建立的提供者 (PA) 邏輯網路，透過網路控制站，以對應至其實體網路拓撲。 網路控制站會從主機連線的位置的邏輯子網路的 IP 首碼配置兩個 PA IP 位址。 網路控制站也會指出適當的 VLAN 標記，要套用的 IP 位址。  

然後使用網路控制站，Contoso Corp 和 Fabrikam Corp 建立其虛擬網路和子網路，而由主機服務提供者所指定的提供者 (PA) 邏輯網路。 Contoso Corp 和 Fabrikam Corp 都各自將 SQL Server 和網頁伺服器移到相同裝載提供者的共用 IaaS 服務，且剛好都在 Hyper-V 主機 1 上執行 **SQL** 虛擬機器，以及在 Hyper-V 主機 2 上執行 **Web** (IIS7) 虛擬機器。 所有虛擬機器都會保留原始的內部網路 IP 位址 (它們的 CA)。  

這兩家公司是由網路控制站指派下列虛擬子網路識別碼 (VSID)，如下所示。  在每個 HYPER-V 主機上的 「 主機代理程式 」 從網路控制站接收已配置的 PA IP 位址，並建立非預設的網路區間中的兩個 PA 主機 Vnic。 網路介面指派給每個這些主機 Vnic，PA IP 位址指派，如下所示：  

-   Contoso Corp 虛擬機器的 VSID 和 Pa:**VSID**是 5001、 **SQL 的 PA**是 192.168.1.10、 **Web 的 PA**是 192.168.2.20  

-   Fabrikam Corp 虛擬機器的 VSID 和 Pa:**VSID**是 6001、 **SQL 的 PA**是 192.168.1.10、 **Web 的 PA**是 192.168.2.20  

網路控制卡會連接到 SDN 主機代理程式將維持 （在 OVSDB 資料庫資料表中） 的持續性存放區中的原則 （包括 CA-PA 對應） 的所有網路原則。  

當 HYPER-V 主機 2 上的 Contoso Corp Web 虛擬機器 (10.1.1.12) 建立在 10.1.1.11 的 SQL Server 的 TCP 連接時，會發生下列情況：  

-   10.1.1.11 的目的地 MAC 位址的 VM Arp  

-   在 vSwitch VFP 擴充功能會攔截此封包，並將它傳送至 SDN 的主機代理程式  

-   SDN 的主機代理程式會尋找在 10.1.1.11 的 MAC 位址為其原則存放區  

-   如果找到 MAC，則主機代理程式會插入至 VM 的 ARP 回應  

-   如果找不到 MAC，沒有回應，並在 10.1.1.11 的 VM 中的 ARP 項目標示無法連線。  

-   VM 現在會建構包含正確 CA 乙太網路和 IP 標頭的 TCP 封包，並將它傳送至 vSwitch  

-   轉送擴充功能中的 vSwitch VFP 處理此封包透過 VFP 層級 （如下所述） 指派給來源 vSwitch 連接埠的封包已收到，VFP 整合的流程資料表中建立新的流程-項目  

-   VFP 引擎執行每個圖層 （例如虛擬網路層） 的 IP 和乙太網路標頭為基礎的規則比對或流程資料表查閱。  

-   在虛擬網路層級中相符的規則參考的 CA-PA 對應空間，並執行封裝。  

-   VSID 以及 VNet 層中指定的封裝類型 （VXLAN 或 NVGRE）。  

-   如果 VXLAN 封裝，VSID 5001 VXLAN 標頭中建構的外部 UDP 標頭。  
    指派給 HYPER-V 主機 2 (192.168.2.20) 和 HYPER-V 主機 1 (192.168.1.10) 分別根據 SDN 主機代理程式的原則存放區的來源與目的地 PA 位址被建構外部的 IP 標頭。  

-   PA 路由中的圖層 VFP 再流向此封包。  

-   VFP 中的之 PA 路由層會參考用於 PA 空間流量，以及 VLAN ID 的網路區間，並使用主應用程式的 TCP/IP 堆疊正確將 PA 封包轉送到 HYPER-V 主機 1。  

-   收到封裝的封包的詳細資訊，HYPER-V 主機 1 PA 網路區間中接收的封包，並將它轉送到 vSwitch。  

-   VFP 會處理透過其 VFP 層封包，與 VFP 整合的流程資料表中建立新的流程-項目。  

-   VFP 引擎比對中的虛擬網路圖層的 ingres 規則，並移除外部封裝的封包的乙太網路、 IP 及 VXLAN 標頭。  

-   VFP 引擎接著會轉送封包的目的地 VM 所連線的 vSwitch 連接埠。  

A similar process for traffic between the Fabrikam Corp **Web** 和 **SQL** 虛擬機器間流量類似的程序會針對 Fabrikam Corp. 使用 HNV 原則設定。結果，Fabrikam Corp 和 Contoso Corp 虛擬機器透過 HNV 進行互動，如同在原始的內部網路一般。 即使它們使用相同的 IP 位址，也絕對無法彼此互動。  

個別的位址 （Ca 和 Pa）、 HYPER-V 主機的原則設定和 CA 之間的流量輸入和輸出虛擬機器的 PA 位址轉譯隔離這些伺服器使用 NVGRE 金鑰或 VLXAN VNID 組。 此外，虛擬化對應和轉換會分隔實體網路基礎架構與虛擬網路架構。 即使 Contoso **SQL** 和 **Web** 及 Fabrikam **SQL** 和 **Web** 都位於它們自己的 CA IP 子網路 (10.1.1/24) 內，其實體部署都發生在不同 PA 子網路 192.168.1/24 和 192.168.2/24 各自的主機上。 這暗示跨子網路的虛擬機器佈建和即時移轉可以透過 HNV 執行。  

## <a name="hyper-v-network-virtualization-architecture"></a>Hyper-V 網路虛擬化架構  
在 Windows Server 2016 中，HNVv2 是使用 Azure 虛擬篩選平台 (VFP) 也就是 HYPER-V 交換器內的 NDIS 篩選擴充功能來實作。 VFP 的重要概念是使用內部 API 公開 SDN 主機代理程式的程式設計網路原則的相符項目動作資料流程引擎。 SDN 主機代理程式本身 OVSDB 和 WCF SouthBound 溝通管道透過網路控制站從接收網路原則。 不只是虛擬網路原則 (例如 CA-PA 對應) 使用 VFP，但其他的原則，例如 Acl、 QoS 和等等。  

VSwitch 與 VFP 轉送擴充功能的物件階層架構如下所示：  

-   vSwitch  

    -   外部 NIC 的管理  

    -   NIC 硬體卸載  

    -   全域的轉送規則  

    -   連接埠  

        -   轉送的線釘選的圖層的輸出  

        -   對應和 NAT 集區的空間清單  

        -   整合的流程資料表  

        -   VFP 層  

            -   流程資料表  

            -   群組  

            -   規則  

                -   規則可以參考空間  

VFP，在圖層會建立每個原則類型 （例如，虛擬網路），並是一組標準的規則/流程資料表。 特定的規則時，會指派給該圖層上，來實作這類功能之前，它並沒有任何內建的功能。 每個圖層指派優先順序，圖層時，會指派給連接埠上，依遞增的優先順序。 規則會組織成群組，主要是根據方向和 IP 位址系列。 群組也會指派優先順序，最多會從群組的一個規則可比對指定的流程。  

與 VFP 擴充功能的 vSwitch 的轉送邏輯如下所示：  

-   輸入處理 （從觀點來看，封包傳送至連接埠的輸入）  

-   轉送  

-   輸出處理 （從觀點來看，封包保留在連接埠的輸出）  

VFP 支援 NVGRE 和 VXLAN 封裝類型，以及外部 MAC VLAN 架構轉送的內部 MAC 轉送。  

VFP 擴充功能具有緩慢路徑和封包周遊的快速路徑。 在流程中的第一個封包必須周遊中每個圖層的所有規則群組，並執行規則的作業成本很高的查閱。 不過，一旦流程整合的流程資料表與 （根據比對的規則） 的動作清單中註冊所有後續封包將會處理根據統一的流程資料表項目。  

HNV 原則是由 「 主機代理程式所編寫。 每個虛擬機器網路介面卡的 IPv4 位址設定。 虛擬機器將使用 CA 來彼此通訊，而它們會存在於來自虛擬機器的 IP 封包中。 HNV 封裝儲存在 「 主機代理程式 」 的資料庫中的網路虛擬化原則為基礎的 PA 框架的 CA 框架。  

![HNV 架構](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  

圖 9：HNV 架構  

## <a name="summary"></a>總結  
雲端式資料中心可以提供許多優點，例如，改善的延展性和較佳的資源使用率。 若要了解這些潛在優點，需要具備基本上可以在動態環境中處理多組織用戶共享延展性問題的技術。 HNV 的設計就是要解決這些問題，同時透過分離實體網路拓撲的虛擬網路拓撲來提升資料中心的運作效率。 建立以現有的標準，HNV 會在現今的資料中心中執行，並與您現有的 VXLAN 基礎結構運作。 HNV 的客戶可立即將其資料中心合併到私人雲端，或順暢地延伸其資料中心，以使用混合式雲端的主機伺服器提供者的環境。  

## <a name="BKMK_LINKS"></a>另請參閱  
若要深入了解 HNVv2，請參閱下列連結：  


|       內容類型       |                                                                                                                                              參考                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **社群資源**  |                                                                -   [私用雲端架構部落格](https://blogs.technet.com/b/privatecloud)<br />-詢問問題： [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **RFC**          |                                                                   -   [NVGRE Draft RFC](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **相關的技術** | -若為 Windows Server 2012 R2 中 HYPER-V 網路虛擬化技術詳細資訊，請參閱[HYPER-V 網路虛擬化技術詳細資料](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [網路控制站](../../../sdn/technologies/network-controller/Network-Controller.md) |

