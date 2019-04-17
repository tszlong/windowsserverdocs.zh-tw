---
title: HYPER-V 網路模擬技術的詳細資料中 Windows Server 2016
description: 本主題提供 HYPER-V 網路模擬 Windows Server 2016 中相關技術的資訊
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: af2b2a0b151601124bb473c465e7d5a97f2b9150
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>HYPER-V 網路模擬技術的詳細資料中 Windows Server 2016

>適用於：Windows Server 2016

伺服器模擬可在單一實體主機; 同時執行多個伺服器執行個體尚彼此隔離的伺服器執行個體。 每個一樣基本上運作為是否唯一實體電腦上執行的伺服器。  
  
網路模擬提供類似的功能、 中的多個 virtual 網路 （潛在重疊的 IP 位址） 執行相同的實體網路基礎結構及每個 virtual 網路運作為是否只 virtual 網路上的共用的網路基礎結構執行。 圖 1 顯示此關係。  
  
![伺服器模擬與網路模擬](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
圖 1： 伺服器模擬與網路模擬  
  
## <a name="hyper-v-network-virtualization-concepts"></a>HYPER-V 網路模擬概念  
在 [HYPER-V 網路模擬 (HNV)，客戶承租人定義或 」 的擁有者 」 設定的 IP 子網路中的企業資料中心部署。 客戶可以的公司或多個部門或私人資料中心業務單位需要隔離的網路或房客在公用資料中心裝載服務提供者的企業。 每個客戶只能有一或多[虛擬網路](#VirtualNetworks)資料中心，與每個 virtual 網路所組成一或多[虛擬子網路](#VirtualSubnets)。  
  
有兩種 HNV 實作，將可在 Windows Server 2016: HNVv1 和 HNVv2。  
  
-   **HNVv1**  
  
    HNVv1 是與 Windows Server 2012 R2 和系統中心 2012 R2 一樣 Manager (VMM) 相容。 設定 HNVv1 依賴 WMI 管理及 Windows PowerShell cmdlet （透過 System Center VMM 加速） 定義隔離設定和客戶位址 (CA) virtual 網路的實際地址 (PA) 對應和路由。 若要在 Windows Server 2016 HNVv1 新增了任何其他功能，並計劃中的新功能。  
    
    • 設定小組，HNV V1 不相容的平台。
    
    若要使用哈 NVGRE 閘道使用者 o 需要任一使用 LBFO 小組或不團隊。 或
    
    O 使用網路控制器部署閘道的設定，以切換。

  
-   **HNVv2**  
  
    使用 Azure Virtual 篩選平台 (VFP) 轉寄切換 HYPER-V 中的擴充功能是實作 HNVv2 包含大量的新功能。 Microsoft Azure 堆疊的軟體定義網路 (SDN) 堆疊包含新的網路控制器完全整合 HNVv2。  透過 Microsoft virtual 的網路原則定義[Network Controller](../../../sdn/technologies/network-controller/Network-Controller.md)使用 RESTful NorthBound (NB) API 並套用到透過多個 SouthBound Intefaces (SBI) 包括 OVSDB 主機代理程式。 主機代理程式 VFP 擴充 HYPER-V 開關切換至何處執行的原則。  
  
    > [!IMPORTANT]  
    > 本主題焦某 HNVv2。  
  
### <a name="VirtualNetworks"></a>Virtual 網路  
  
-   每個 virtual 網路一或多個 virtual 子網路所組成。 Virtual 網路形成隔離邊界虛擬機器 virtual 網路位置只能與其他通訊。 一般而言，這種隔離已執行使用 Vlan 隔離的 IP 位址和 802.1q 標記或 VLAN id。 但 HNV，使用隔離執行使用 NVGRE 或 VXLAN 封裝重疊的 IP 子網路針對或 tenants 之間的可能性建立覆疊網路。  
  
-   每個 virtual 網路主機上具有獨特路由網域 ID (RDID)。 這個 RDID 大約地圖資源 id 找出 virtual 網路 Network Controller 中的其餘部分資源。 參考統一資源識別碼 (URI) 命名空間使用附加資源 ID virtual 網路其餘資源  
  
### <a name="VirtualSubnets"></a>Virtual 子網路  
  
-   子網路 virtual 實作虛擬電腦的層級 3 IP 子網路語意相同 virtual 子網路中。 子網路 virtual 形成廣播的網域 （類似 VLAN） 和隔離由使用 NVGRE 承租人網路 ID (TNI) 或 VXLAN 網路識別碼 (VNI) 欄位。  
  
-   每個 virtual 子網路屬於單一 virtual 網路 (RDID)，而已指派唯一 Virtual 子網路 ID (VSID) 使用 TNI 或 VNI 鍵頭封裝封包。 VSID 必須唯一 datacenter 和 4096 到 2 的範圍中 ^24-2。  
  
主要優點 virtual 網路與路由網域是雲端它可以讓針對讓他們自己的網路拓撲 （例如 IP 子網路）。 圖 2 所示示範，以 Contoso Corp 擁有兩個不同的網路，R 與網路 D 和銷售網路。 這些網路有不同的尚網域 Id，因為它們無法彼此互動。 也就是，以 Contoso R 與 D 網路隔離從網路 Contoso 銷售即使是兩者由 Contoso Corp.Contoso R 所擁有與網路 D 包含三 virtual 子網路。 請注意，RDID 和 VSID 唯一資料中心。  
  
![客戶網路和 virtual 子網路](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
圖 2 所示： 客戶網路和 virtual 子網路  
  
**轉送層級 2**  
  
在圖 2 所示，在 VSID 5001 虛擬電腦都可以有轉送虛擬機器 VSID 5001 HYPER-V 開關切換至透過在他們封包。 從一樣 VSID 5001 中輸入封包會傳送至上 HYPER-V 開關切換至特定 VPort。 透過這些封包 HYPER-V 開關切換至套用輸入規則 （例如壓縮） 和對應 （例如封裝標頭）。 封包然後轉送至不同 VPort HYPER-V 切換上 （如果目的地一樣已連接到同一部主機） 或其他 HYPER-V 切換在不同的主機 （如果有不同的主機上位於目的地一樣）。  
  
**層級 3 路由**  
  
同樣地，在 VSID 5001 虛擬電腦可能路由傳送至 VSID 5002 或 VSID 5003 虛擬電腦 HNV 分散式路由器會在每個 HYPER-V 主機的 VSwitch 他們封包。 於封包傳遞至 HYPER-V 切換、 HNV 更新的目的地一樣 VSID VSID 傳入封包。 這只會發生這兩個 VSIDs 相同 RDID 如果。  因此，virtual 網路介面卡，與 RDID1 無法傳送封包 virtual 網路介面卡，與 RDID2 不通過閘道。  
  
> [!NOTE]  
> 封包流程描述以上，在 「 一樣 「 實際上是指 virtual 網路介面卡上一樣。 常見案例是一樣僅有單一 virtual 網路介面卡。 在本案例中的文字 」 一樣 」 和 「 virtual 網路介面卡] 概念可以表示是同一件事。  
  
每個 virtual 子網路定義層級 3 IP 子網路和層級 2 (L2) 廣播的網域邊界 VLAN 類似。 當一樣廣播了封包時，HNV 使用單複寫 (UR) 做了一份原始封包及每個 VM 的位址相同 VSID 中有哪些取代目的地 IP 和 MAC。  
  
> [!NOTE]  
> Windows Server 2016 出貨時, 將會使用單複寫實作廣播和子網路多點傳送。 子網路跨多點路由並 IGMP 不支援。  
  
正在廣播的網域，除了 VSID 提供隔離。 在 HNV virtual 網路介面卡被連接至 HYPER-V 切換具有 ACL 規則套用直接連接埠 （資源 virtualNetworkInterface 其餘部分） 或是其中一部分 virtual 子網路 (VSID)。  
  
HYPER-V 切換連接埠必須套用 ACL 規則。 此 ACL 可能會允許所有、 拒絕全部或更多針對只允許特定類型的資料傳輸根據 5-tuple 來源 IP、 目的地 IP、 來源連接埠，目的地連接埠 (通訊協定） 相符。  
  
> [!NOTE]  
> HYPER-V 切換擴充功能不適用於 HNVv2 中新的軟體定義網路 (SDN) 堆疊。 使用 Azure Virtual 篩選平台 (VFP) 切換擴充功能無法使用的任何其他第 3 廠商切換延伸搭配實作 HNVv2。  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>切換，並在網路 HYPER-V 模擬路由  
HNVv2 實作正確層級 2 (L2) 切換和層級 3 (L3) 路由語意如同實體切換工作，或路由器會運作。 當一樣連接到 HNV virtual 網路會嘗試相同 virtual 子網路 (VSID) 以了解 CA 的 MAC 位址遠端一樣必須先在連接的另一個一樣。 如果有 ARP 項目的一樣的來源一樣的 ARP 表格中的 IP 位址，使用此項目從 MAC 位址。 如果輸入不存在，將會傳送給 ARP 廣播傳回目的地一樣的 IP 位址相對應的 MAC 位址，要求來源一樣。 HYPER-V 開關切換至將攔截這個要求，並將它傳送給主機代理程式。 主機代理程式看起來會要求的目的地一樣的 IP 位址相對應的 MAC 位址其本機資料庫中。  
  
> [!NOTE]  
> 主機代理，做為 OVSDB 伺服器，使用 VTEP 結構描述 variant 儲存 CA-PA 對應、 MAC 表格和等等。  
  
如果有可用的 MAC 位址，主機代理程式插入 ARP 回應與傳送此回到一樣。 一樣的網路堆疊所有所需的 L2 標頭資訊後，畫面會傳送到對應 HYPER-V 連接埠 V 式開關切換至。 內部，HYPER-V 開關切換至測試此框架有 N 序元組符合規則指派給 V 連接埠和適用於特定的轉換框架根據本規則。 最重要的是一組封裝轉換適用於建構封裝標頭 NVGRE 或 VXLAN，使用定義在 Network Controller 的原則。 根據原則主機代理程式所設計，CA-PA 對應用來判斷 HYPER-V 主機的 IP 位址所在目的地一樣。 HYPER-V 開關切換至確保正確的路徑規則及 VLAN 標籤到達遠端 PA 地址，會套用至外部封包。  
  
連接到 HNV virtual 網路一樣想要建立連接一樣，在不同的 virtual 子網路 (VSID) 使用時，如果需要路由傳送，因此請務必妥善封包。 HNV 假設星形拓撲其中瑞曲之戰所有 IP 首碼 （意義一個預設之前的路徑日閘道） 做為躍 CA 空間中只有一個 IP 位址。 目前，這會執行單一預設路由的限制和路徑非預設不支援。  
  
### <a name="routing-between-virtual-subnets"></a>子網路 Virtual 之間路由  
實體網路，在的 IP 子網路是位置 （virtual 和實體） 的電腦可以直接彼此層級 2 (L2) 網域。 L2 網域是廣播的網域位置 ARP 項目 （IP:MAC 位址地圖） 的所有介面正在廣播的 ARP 要求透過學習和 ARP 回應會傳送到要求主機。 電腦完全建構包括乙太網路標頭 L2 框架使用以來 ARP 回應的 MAC 資訊。 不過，如果在不同的 L3 子網路的 IP 位址，ARP 要求不會跨此 L3 邊界。 而是來源子網路中的 IP 位址 L3 路由器介面 （躍或預設閘道） 必須回應有它自己的 MAC 位址這些 ARP 要求。  
  
在標準 Windows 網路，系統管理員可以建立靜態路徑，然後指定這些網路介面。 此外，[預設閘道] 通常被設定為上封包預設路由 (0.0.0.0 / 0) 直接傳送位置介面躍 IP 位址。 如果不有任何特定路由預設閘道會收到封包。 這通常是您的實體網路路由器。  HNV 使用建路由器是每個主機的一部分，並在每個 VSID 建立分散式的路由器的 virtual 網路介面。  
  
因為 HNV 前提星級拓撲 HNV 分散式的路由器會做為單一預設閘道之間的相同 VSID 網路 Virtual 子網路將所有資料傳輸。 地址做為預設閘道預設值是在 VSID 最低的 IP 位址，以及已指派給 HNV 分散式路由器。 允許此分散式的路由器 VSID 網路中的所有資料傳輸方式因為每一部主機可以直接傳送資料傳輸到適當的主機而不需要介正確傳遞至分公司。  尤其是時的相同 VM 網路但不同 Virtual 子網路中的兩個虛擬電腦上相同的實體主機。  您將會在本區段中稍後看到、 封包從未必須離開實體主機。  
  
### <a name="routing-between-pa-subnets"></a>路由之間 PA 子網路  
相較於配置一個 PA IP 位址的每個 Virtual 子網路 (VSID) HNVv1，HNVv2 現在可以使用每個 Switch-Embedded 小組 （設定） NIC 小組成員 PA IP 位址。 預設部署假設兩-NIC 團隊，並會指定每個主機的兩個 PA IP 位址。 一部主機有 PA IPs 指派相同提供者 (PA) 邏輯子網路相同的 VLAN 上。 在相同的 virtual 子網路中的兩個承租人 Vm 確實位於兩個不同的主機這兩個不同的提供者邏輯子網路來連接。 HNV 將建構封裝封包根據 CA-PA 對應的外部 IP 標頭。 不過，它依賴 PA 預設閘道 ARP 主機 TCP/IP 堆疊，然後根據 ARP 回應的外部乙太網路標頭的組建。 一般而言，此 ARP 回應來自 SVI 介面實體切換或 L3 路由器上已連接主機的地方。 HNV 因此依賴路由加密的封包提供者邏輯子網路之間 L3 路由器日 Vlan。  
  
### <a name="routing-outside-a-virtual-network"></a>路由外 Virtual 網路  
大多數客戶部署需要從 HNV 環境通訊不 HNV 環境的部分資源。 網路模擬閘道需要允許兩個環境間通訊。 需要 HNV 閘道基礎結構包含的私人雲端和混合雲端。 基本而言，HNV 閘道所需的層級 3 路由內外 （實體） 網路 （包括 NAT） 或之間不同的網站和/或雲朵 （私人或公開） 使用 IPSec VPN 或 GRE 的通道。  
  
可以進入實體的不同尺寸規格閘道。 他們可以在 Windows Server 2016 納入的架頂端 (TOR) 切換作為存取透過 Virtual IP (VIP) 來負載平衡器通知 VXLAN 閘道進入其他現有的網路裝置，或可以將新的網路獨立應用裝置上建置。  
  
如需 Windows RAS 閘道選項的相關資訊，請查看[RAS 閘道](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md)。  
  
## <a name="packet-encapsulation"></a>封包封裝  
在 HNV 每個 virtual 網路介面卡是相關聯的兩個 IP 位址：  
  
-   **客戶地址]** (CA) 位址指派客戶，根據其內部網路基礎結構。 此地址可客戶如同它不需要移至公用或私人雲端換貨網路流量的一樣。 已可見一樣，且可以客戶。  
  
-   **提供者地址**(PA) 的 IP 位址裝載者指派或根據其實體網路基礎結構 datacenter 系統管理員。 PA 會顯示在網路上的換貨執行裝載一樣 HYPER-V server 的封包。 PA 會顯示在實體網路，但不是一樣。  
  
Ca 維護顧客網路拓撲，來 PAs 實作分離實際基礎實體網路拓撲和地址，並擬化檔案。 下圖顯示概念和之間的關係一樣 Ca 網路基礎結構 PAs 根據網路模擬。  
  
![網路模擬上方所在的基礎結構的概念圖](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
圖 6： 的網路模擬上方所在的基礎結構的概念圖  
  
在圖表，客戶虛擬電腦傳送資料封包 CA 空間，往返實體網路基礎結構透過自己 virtual 網路或 「 通道 」。 在上述範例通道可以視為 」 信封 」 操作遺漏出貨標籤 （PA 位址） 直接從左邊來源主機傳送到目的主機上向右以 Contoso 和 Fabrikam 資料封包。 關鍵在於主機如何判斷 」 寄送地址] (PA) Contoso Fabrikam CA、 」 信封 」 如何放在封包及如何解除包裝封包並正確地提供 Contoso 和 Fabrikam 目的地虛擬機器目的地主機相對應。  
  
此簡單類比反白顯示網路模擬的重要：  
  
-   每個一樣 CA 對應至 PA.實體主機 可能會有多個相同 PA.相關聯的 Ca  
  
-   虛擬電腦傳送 CA 空間，這進入 「 信封 」 的 PA 來源和目的地配對根據對應資料封包。  
  
-   CA-PA 對應必須允許主機來區分不同客戶虛擬機器封包。  
  
如此一來，是虛擬化虛擬機器所使用的網路位址虛擬化網路的機制。 負責位址對應，網路控制器和主機代理維持使用 MS_VTEP 架構的製圖資料庫。 下一節中描述的實際地址模擬機制。  
  
## <a name="network-virtualization-through-address-virtualization"></a>網路模擬透過模擬地址  
HNV 實作覆疊網路模擬一般路由封裝 (NVGRE) 或虛擬最具擴充性的區域網路 (VXLAN) 使用承租人網路。  預設 VXLAN。  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>Virtual 最具擴充性的區域網路 (VXLAN)  
虛擬最具擴充性的區域網路 (VXLAN) ([RFC 7348](http://www.rfc-editor.org/info/rfc7348)) 通訊協定已被普遍在市場，並可支援從等 Cisco、 錦緞、 Arista、 Dell、 HP 和其他廠商取得。 使用傳輸 UDP VXLAN 通訊協定。 已 VXLAN IANA 指派 UDP 目的連接埠 4789 與 UDP 連接埠來源應該 hash 的資訊，用於 ECMP 分配最封包。 UDP 標頭之後 VXLAN 標頭附加到包括後面 3 位元組欄位的 VXLAN 網路識別碼 (VNI)-VSID-後面另一個保留 1 位元組欄位保留的 4 位數欄位封包。 VXLAN 標頭之後附加原始 CA L2 框架 （不含 CA 乙太網路架構 FCS）。  
  
![VXLAN 封包標頭](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>一般路由壓縮 (NVGRE)  
此網路模擬機制使用一般路由封裝 (NVGRE) 通道標頭的一部分。 在 [NVGRE，一樣的封包封裝在另一封包。 這個新的封包標頭有來源的適當和目的地 PA IP 位址，除了 Virtual 子網路 ID，會儲存在金鑰欄位 GRE 標頭，如圖 7 所示。  
  
![封裝 NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
圖 7 所示： 網路模擬 NVGRE 封裝  
  
子網路 Virtual ID 允許找出為任何特定的封包客戶一樣主機時，可能會重疊即使 PA 的與 CA 的封包。 這可讓所有虛擬電腦上同一部主機分享單一 PA，如圖 7 所示。  
  
分享 PA 延展性網路上有很大的影響。 所需的網路基礎結構學習 IP 和 MAC 位址數目可以大幅降低。 例如，如果每個主機 30 虛擬電腦的 IP 平均且需要學習網路基礎結構的 MAC 位址減少 30.embedded Virtual 子網路中的編號封包倍也可讓輕鬆相互關聯的實際針對封包。  
  
Windows Server 2012 R2 的共用配置 PA 是 VSID 每一個 PA 每個主機。 Windows Server 2016 配置是一個 PA 每個 NIC 小組的成員。  
  
Windows Server 2016 的與更新版本，HNV 完全支援 NVGRE 和 VXLAN 另外;不需要升級，或購買新的網路硬體，例如 Nic （網路介面卡），參數或路由器。 這是因為這些封包來傳送 PA 空間，也就是目前的網路基礎結構相容的一般 IP 封包。  不過，以取得最佳效能使用 Nic 支援支援工作卸載的最新驅動程式。  
  
## <a name="multi-tenant-deployment-example"></a>多承租人部署範例  
下圖顯示兩個針對關聯的網路原則定義 CA-PA 位於雲端的資料中心的部署範例。  
  
![多承租人部署範例](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
圖 8： 多承租人部署範例  
  
請參考圖 8 範例。 移至主機的供應商之前的共用 IaaS 服務：  
  
-   Contoso Corp 執行 SQL Server (名為**SQL**)，IP 位址 10.1.1.11 與 web 伺服器 (名為**網頁**)，其 SQL Server 資料庫交易使用的 IP 位址 10.1.1.12。  
  
-   Fabrikam Corp 執行 SQL Server、 也稱為**SQL**並指定 IP 位址 10.1.1.11，與 web 伺服器，也稱為**網頁**，並也在 10.1.1.12 IP 位址，使用其 SQL Server 資料庫交易。  
  
我們將會假設裝載的服務提供者已先前建立透過依據他們實體網路拓撲 Network Controller 的提供者 (PA) 邏輯網路。 Network Controller 的邏輯子網路的 IP 首碼所定義連接主機的地方配置兩個 PA IP 位址。 網路控制器也會指出適當的 VLAN 標籤套用的 IP 位址。  
  
使用網路控制器，以 Contoso Corp Fabrikam Corp 則建立 virtual 網路和子網路的由裝載的服務提供者所指定的提供者 (PA) 邏輯網路的支援。 移動他們各團隊 Contoso Corp 和 Fabrikam Corp 和網頁伺服器相同裝載提供者共用 IaaS 服務，正巧，執行**SQL** HYPER-V 主機 1 虛擬電腦和**網頁**(IIS7) 虛擬電腦上 HYPER-V 主機 2。 虛擬的所有電腦都維持其原始內部 IP 位址 (他們 Ca)。  
  
這兩個公司已指派下列 Virtual 子網路 ID (VSID) Network Controller 如下所示。  主機上的代理程式每個 HYPER-V 主機接收 Network Controller 的配置的 PA IP 位址，並建立區間非預設網路中的兩個 PA 主機 vNICs。 網路介面已指派給每個這些的主機 vNICs PA IP 位址指派，如下所示：  
  
-   VSID 和 PAs Contoso Corp 虛擬電腦： **VSID**是 5001， **SQL PA**是 192.168.1.10， **Web PA**是 192.168.2.20  
  
-   VSID 和 PAs Fabrikam Corp 虛擬電腦： **VSID**是 6001， **SQL PA**是 192.168.1.10， **Web PA**是 192.168.2.20  
  
Network Controller plumbs 所有的網路原則 （包括 CA-PA 對應），將會維持原則 （以 OVSDB 資料庫表格） 持續市集中 SDN 主機代理程式。  
  
當 Contoso Corp Web 一樣 (10.1.1.12) HYPER-V 主機 2 上建立的 TCP 連接 SQL Server 10.1.1.11 在時下列動作：  
  
-   VM Arp 10.1.1.11 的目的地的 MAC 位址  
  
-   在 vSwitch VFP 擴充功能攔截這封包並將其傳送到 SDN 主機代理程式  
  
-   SDN 主機代理程式看起來 10.1.1.11 的 MAC 位址其原則網上商店中  
  
-   如果找到是 MAC，則主機代理程式插入回 VM ARP 回應  
  
-   如果找不到是 MAC，則會傳送無回應和 ARP 中的項目適用於 10.1.1.11 VM 標示無法存取。  
  
-   VM 現在建構正確 CA 乙太網路及 IP 標頭的 TCP 封包，並將其傳送到 vSwitch  
  
-   轉送 vSwitch 中的擴充功能 VFP 處理這封包透過指派給在其封包接收，以及 VFP 流量統一的表格中建立新的 flow 輸入來源 vSwitch 連接埠 VFP 層 （如下所述）  
  
-   VFP 引擎會執行規則符合或流量表格搜尋 IP 和乙太網路首依據每一層 （例如 virtual 網路層級）。  
  
-   符合 virtual 網路層規則參考 CA-PA 對應空間，並執行封裝。  
  
-   加上 VSID VNet 層中指定封裝類型 （VXLAN 或 NVGRE）。  
  
-   VXLAN 封裝，在外部 UDP 標頭建構 VSID 5001 VXLAN 標頭的使用。  
    指派給 HYPER-V 主機 2 (192.168.2.20) 與 「 HYPER-V 主機 1 (192.168.1.10) 分別根據 SDN 主機代理程式原則市集來源和目的地 PA 位址建構外部 IP 標頭。  
  
-   然後流向 PA 路由層級在 VFP 這封包。  
  
-   在 VFP PA 路由層將參考用於 PA 空間交通和 VLAN ID 網路區間，並使用 TCP/IP 堆疊的主機正確 HYPER-V 主機 1 向前 PA 封包。  
  
-   封裝封包收到 HYPER-V 主機 1 PA 網路槽在收到一封包，轉送給 vSwitch。  
  
-   VFP 處理封包透過其 VFP 層級，並建立 VFP 流量統一的表格中的新流程項目。  
  
-   VFP 引擎符合 virtual 網路層 ingres 規則，除去外部封裝封包乙太網路、 IP 和 VXLAN 標頭。  
  
-   VFP 引擎然後轉送連接目的地 VM 的 vSwitch 連接埠封包。  
  
類似的程序 Fabrikam Corp 間的流量的**網站**和**SQL**虛擬電腦使用 Fabrikam Corp.HNV 原則設定如此一來，Corp HNV，Fabrikam 與 Contoso Corp 虛擬電腦互動當成其原始內部上。 他們可以永遠不會彼此互動，即使在他們使用的相同的 IP 位址。  
  
（Ca 和 PAs） 的另一個地址、 原則設定的 HYPER-V 主機，CA 之間輸入 / 輸出一樣流量 PA 位址轉譯隔離伺服器 NVGRE 鍵或 VLXAN VNID 使用這些的設定。 此外，模擬對應和轉換分離從實體網路基礎結構 virtual 網路架構。 雖然 Contoso **SQL**和**網頁**並 Fabrikam **SQL**和**網頁**位於自己 CA IP 子網路 (10.1.1/24)，其實體部署分別交貨不同 PA 子網路，192.168.1/24 和 192.168.2/24，兩部主機上。 不提供跨子網路一樣和即時移轉成為 HNV 使用。  
  
## <a name="hyper-v-network-virtualization-architecture"></a>HYPER-V 網路模擬架構  
Windows Server 2016 中 HNVv2 係使用 Azure Virtual 篩選平台 (VFP) 也就是在 HYPER-V 開關切換至 NDIS 篩選擴充功能。 主要的 VFP 概念是公開 SDN 主機代理程式的程式設計網路原則內部 api 符合動作流程引擎。 SDN 主機代理本身接收 Network Controller OVSDB 和 WCF SouthBound 的通訊通道上的網路原則。 不只是 virtual 的網路原則 (例如 CA-PA 對應) 設計的額外的原則，例如 Acl、 服務品質、 等等，但 VFP 使用。  
  
以下是物件階層 vSwitch 和 VFP 轉寄擴充功能：  
  
-   vSwitch  
  
    -   外部 NIC 管理  
  
    -   NIC 硬體卸載  
  
    -   全球轉送規則  
  
    -   連接埠  
  
        -   轉送層彈將釘選的輸出  
  
        -   列出的對應和 NAT 集區的空間  
  
        -   流量統一的表格  
  
        -   VFP 層  
  
            -   流量表格  
  
            -   群組  
  
            -   規則  
  
                -   規則參考空間  
  
在 VFP，層建立每個原則類型 （例如，Virtual 網路），是一組一般規則日流程資料表。 它不會有任何建功能直到特定規則指派給該層實作此類功能。 每個層級指派優先順序和層級已指派給連接埠遞增優先順序。 規則分為群組主要根據方向及 IP 位址的家庭。 擁有也群組高優先順序，並從群組一個規則最多可以符合指定的流程。  
  
副檔名 VFP vSwitch 轉接邏輯如下：  
  
-   輸入處理 （從進入連接埠封包觀點輸入）  
  
-   轉接  
  
-   輸出處理 （從封包離開連接埠觀點輸出）  
  
VFP 支援 NVGRE 和 VXLAN 封裝類型以及外部 MAC VLAN 根據轉接最 MAC 轉送。  
  
VFP 擴充功能有 slow 路徑和封包周遊快速路徑。 第一封包流程必須往返每個層級在所有規則群組，以及執行規則對應即高。 不過之後流量統一如下表所使用的動作 （根據規則符合） 清單係流程, 所有後續封包將處理根據流量統一的表項目。  
  
HNV 原則是設計用來主機代理程式。 每個一樣網路介面卡的 [IPv4 位址設定。 這些虛擬的電腦將會使用與互相溝通 Ca 與他們執行中的虛擬電腦的 IP 封包。 HNV 封裝 CA 框架根據主機代理程式資料庫中儲存的網路模擬原則 PA 畫面中。  
  
![HNV 架構](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
圖 9: HNV 架構  
  
## <a name="summary"></a>摘要  
以雲端為基礎的資料中心可提供改善的延展性和得更好的資源使用量許多好處。 若要實現優點可能需要一種技術，徹底位址動態環境中的多承租人擴充性的問題。 HNV 是設計用來處理這些問題，而且也聯繫實體網路拓撲 virtual 網路拓撲改進操作資料中心的效能。 在現有標準上建置，HNV 在今天的資料中心執行，並使用您現有的 VXLAN 基礎結構的運作方式。 針對使用 HNV 可以現在入私人雲端整合他們資料中心或順暢延長他們主機伺服器供應商的環境與混合雲端的資料中心。  
  
## <a name="BKMK_LINKS"></a>也了  
若要了解詳細 HNVv2 查看下列連結：  
  
|內容類型|資訊尋找參考資料|  
|----------------|--------------|  
|**社群資源**|-   [私人雲端架構部落格](http://blogs.technet.com/b/privatecloud)<br />-詢問問題：[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [NVGRE 草稿 RFC](http://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN-RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**相關的技術**|-適用於 Windows Server 2012 R2 HYPER-V 網路模擬技術詳細資訊，請查看[HYPER-V 網路模擬技術的詳細資料](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Network Controller](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


