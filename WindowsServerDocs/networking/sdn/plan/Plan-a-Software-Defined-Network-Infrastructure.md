---
title: 軟體定義的網路基礎結構計劃
description: 本主題提供如何計劃軟體定義網路 (SDN) 基礎結構部署的資訊。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb3b9313996637fa5ee7367c538fe04d7cbefea9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="plan-a-software-defined-network-infrastructure"></a>軟體定義的網路基礎結構計劃

>適用於：Windows Server（以每年次管道）、Windows Server 2016

檢視的下列資訊，以協助計劃軟體定義網路 (SDN) 基礎結構部署。 您檢視這項資訊之後，請查看[部署軟體定義網路基礎結構，](../deploy/Deploy-a-Software-Defined-Network-Infrastructure.md)如部署的資訊。

>[!NOTE]
>本主題中，除了下列 SDN 計劃 content 使用。  
>
> - [安裝和部署 Network Controller 準備需求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  

資訊有關 HYPER-V 網路模擬 (HNV)，您可以用來虛擬化 Microsoft SDN 部署網路，請查看[HYPER-V 網路模擬](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)。  

## <a name="prerequisites"></a>必要條件
本主題描述一些硬體和軟體的必要條件，包括：

-   **實體網路**  
    您需要存取實體網路裝置設定 Vlan、路由、BGP 資料中心橋接 (ETS) 使用 RDMA 技術，如果和資料中心橋接 (PFC) 使用 RoCE 如果為 RDMA 技術。 本主題層級 3 參數上顯示手動切換設定，以及對等 BGP 日路由器或路由並遠端存取伺服器 (RRAS) 一樣。   

-   **實體運算主機**  
這些主機執行 HYPER-V，才能主機 SDN 基礎結構和承租人虛擬電腦。  為獲得最佳效能，稍後中所述的下列主機中需要特定網路硬體**網路硬體**一節。  
      
  
## <a name="physical-network-configuration"></a>實體網路設定

每個實體運算主機需要網路連接到實體切換連接埠來連接一或多個網路介面卡。 網路可以選擇備份層級 2 多個網路邏輯區段分成[VLAN](https://en.wikipedia.org/wiki/Virtual_LAN)。 IP 子網路首碼和 VLAN Id 如下所示範例，而且必須自訂您的環境根據從您的網路系統管理員指導方針。 如果有任何的邏輯網路標記或處於存取模式 VLAN ID 0 這些網路設定時，使用的邏輯子網路中 System Center 一樣 Manager 或 PowerShell 指令碼設定檔。

>[!IMPORTANT]
>Windows Server 2016 軟體定義網路支援 IPv4 位址底圖和覆疊。 不支援 IPv6。
  
### <a name="management-and-hnv-provider-logical-networks"></a>管理和 HNV 提供者邏輯網路

所有實體都計算主機需要邏輯管理網路和 HNV 提供者邏輯網路的存取權。 如果邏輯網路使用 Vlan，必須連接到 trunked 切換連接埠，這些 Vlan 存取實體運算主機。 同樣地，運算主機上的實體網路介面卡不能啟動任何 VLAN 篩選。 如果您正在使用 Switch-Embedded 小組（設定），並在您的主機運算有多個 NIC 小組成員（亦即網路介面卡），您必須連接所有 NIC 小組成員該特定主機相同層級 2 廣播網域。  
  
規劃用途的 IP 位址，每個實體運算主機必須管理邏輯網路從指定至少一個 IP 位址。 網路控制器會自動指派完全兩個 IP 位址，從 HNV 提供者邏輯網路。 如果實體運算主機執行的其他基礎結構虛擬電腦（例如，Network Controller、SLB 日 MUX 或閘道）該主機必須為每個裝載的基礎結構虛擬電腦指派管理邏輯網路從其他 IP 位址。   
  
此外，每個 SLB 日 MUX 基礎結構一樣必須保留 HNV 提供者邏輯網路的 IP 位址。 

>[!IMPORTANT]
>這些 SLB 日 MUX IP 位址必須從指定外 HNV 提供者邏輯網路設定的 IP 位址集區。 若要這樣做可能會導致重複您網路上的 IP 位址。 

Network Controller 需要從做為的其餘部分 IP 位址管理網路保留地址。 您必須手動建立主機 A 記錄 DNS 中的其餘部分 IP 位址。  
  
DHCP 伺服器可以自動指派用於管理網路的 IP 位址，或是您以手動方式可以指定靜態 IP 位址。 會自動 SDN 堆疊會從指定透過，並由 Network Controller IP 集區的個人 HYPER-V 主機的指派 HNV 提供者網路的 IP 位址。   
  
Fabric 系統管理員靜態指派 SLB 日 MUX 透過 PowerShell 指令碼或 VMM 使用 HNV 提供者 IP 位址。 Network Controller 實體運算主機指派 HNV 提供者 IP 位址只之後網路控制器主機代理程式接收網路原則的特定承租人一樣。  
  
#### <a name="sample-network-topology"></a>範例網路拓撲

自訂子網路首碼、VLAN Id 和閘道 IP 位址，根據您的網路系統管理員指導方針。  
  
網路的名稱|子網路|面具|在主幹 VLAN ID|閘道|保留<br />（範例）  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**管理**|10.184.108.0|24|7|10.184.108.1|10.184.108.1-路由器<br /><br />10.184.108.4-network Controller<br /><br />10.184.108.10-運算主機 1<br /><br />10.184.108.11-運算主機 2<br /><br />10.184.108.X-運算主機 X  
|**HNV 提供者**|10.10.56.0|23|11|10.10.56.1|10.10.56.1-路由器<br /><br />10.10.56.2-MUX1 SLB 日  
  
### <a name="logical-networks-for-gateways-and-the-software-load-balancer"></a>適用於閘道和軟體負載平衡器邏輯網路
  
其他邏輯網路需要建立並提供閘道和 SLB 使用量。 同樣地，您需要使用您的網路管理員以取得正確的 IP 首碼、VLAN Id 和閘道 IP 位址這些網路。

#### <a name="transit-logical-network"></a>轉送邏輯網路
  
RAS 閘道和 SLB 日 MUX 使用轉送邏輯網路 BGP 等資訊和北日南（外部-內部）承租人流量換貨。 子網路中的大小通常是小型比其他人。 RAS 閘道或 SLB 日 MUX 虛擬電腦執行的實體運算主機需要有連接到使用這些 Vlan trunked 使用者且無障礙子網路上的計算主機的網路介面卡連接切換連接埠。 每個 SLB 日 MUX 或 RAS 閘道一樣靜態指派一個 IP 位址的轉送邏輯網路。

#### <a name="public-vip-logical-network"></a>公開 VIP 邏輯網路  
  
必須具有路由雲端環境（通常是網際網路路由）以外的 IP 子網路首碼公用 VIP 邏輯網路。  這會用外部用來存取包括前端 VIP 網站-閘道 virtual 網路中的資源前端 IP 位址。

#### <a name="private-vip-logical-network"></a>私人 VIP 邏輯網路
  
不需要為它用於 Vip 只從內部雲端，例如 SLB 管理程式或服務私人存取會路由以外雲端式邏輯 VIP 私人網路。
  
#### <a name="gre-vip-logical-network"></a>GRE VIP 邏輯網路

子網路在於僅供定義 Vip 指派給閘道虛擬電腦執行 S2S GRE 連接類型您 SDN fabric GRE VIP 網路。 不需要在您的路由器或實體參數預先設定並需要不需要指定 VLAN 此網路。   

### <a name="sample-network-topology"></a>範例網路拓撲

自訂子網路首碼、VLAN Id 和閘道 IP 位址，根據您的網路系統管理員指導方針。  
  
網路的名稱|子網路|面具|在主幹 VLAN ID|閘道|保留<br />（範例）  
----------------|----------|--------|--------------------|-----------|-----------------------------  
|**移動**|10.10.10.0|24|10|10.10.10.1|10.10.10.1-路由器  
|**公開 VIP**|41.40.40.0|27|NA|41.40.40.1|41.40.40.1-路由器<br /> 41.40.40.2-SLB 日 MUX VIP<br />41.40.40.3-IPSec S2S VPN VIP  
|**私人 VIP**|20.20.20.0|27|NA|20.20.20.1|20.20.20.1-預設 GW（路由器）  
|**GRE VIP**|31.30.30.0|24|NA|31.30.30.1|31.30.30.1-預設 GW|  
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>邏輯網路所需的 RDMA 為基礎的儲存空間  
  
如果您是使用 RDMA 根據儲存空間，然後您將需要在您運算與儲存空間的主機定義 VLAN 和子網路的每個實體的介面卡。 通常您將有兩個實體的介面卡每個節點此組態。  
  
> [!IMPORTANT]  
> 最實體參數需要 RDMA 標記以便品質服務設定正確套用 VLAN 上傳送的資料傳輸。  無法將 RDMA 資料傳輸到標記 VLAN 或實體存取模式連接埠。  
  
網路的名稱  |子網路  |面具  |在主幹 VLAN ID  |閘道  |保留<br />（範例）    
---------|---------|---------|---------|---------|---------  
**Storage1**     |    10.60.36.0     | 25        |   8      |  10.60.36.1       |  10.60.36.1-路由器<br />10.60.36.x-運算主機 x<br />10.60.36.y-運算主機 y<br />10.60.36.v-運算叢集<br />10.60.36.w-儲存叢集  
|**Storage2**|10.60.36.128|25|9|10.60.36.129|10.60.36.129-路由器<br />10.60.36.x-運算主機 x<br />10.60.36.y-運算主機 y<br />10.60.36.v-運算叢集<br />10.60.36.w-儲存叢集  
  
適用於設定選項的相關詳細資訊，請查看**設定範例**一節。  
  
## <a name="routing-infrastructure"></a>尚基礎結構  
  
如果您要部署使用指令碼，管理 HNV 提供者、傳輸、SDN 基礎結構和 VIP 子網路，必須路由彼此實體網路上。     
  
路由資訊 \ (例如 hop\ 下一步) 的 VIP 子網路 SLB 日 MUX 和 RAS 閘道通知進入實體網路使用內部 BGP 對等。 VIP 邏輯網路不需要指定 VLAN，且不會預先設定的層級 2 切換（例如上架狀切換）。  
  
您需要使用 SDN 基礎結構收到通知 SLB 日 MUXes 和 RAS 閘道 VIP 邏輯網路路徑路由器上建立的 BGP 對等。 BGP 外面只需要發生（從 SLB 日 MUX 或 RAS 閘道外部 BGP 等）的方式。  上方的第一個層路由，您可以使用靜態路徑或其他動態路由通訊協定 OSPF，例如不過，上文所述，VIP 邏輯網路的 IP 子網路首碼執行需要會路由傳送至外部 BGP 等實體網路的。   
  
BGP 外面通常會在受管理的切換或網路基礎結構的一部分路由器設定。 BGP 對等也可能會在 Windows Server 的遠端存取伺服器 (RAS) 角色路由僅模式安裝設定。 必須設定網路基礎結構此 BGP 路由器等其 ASN 及允許從已指派給 SDN 元件 ASN 對等 \（SLB 日 MUX 和 RAS Gateways\）。 您必須從您的實體路由器，或該路由器控制網路系統管理員取得下列資訊：

- 路由器 ASN  
- 路由器 IP 位址  
- ASN 使用 SDN 元件（可從 [私人 ASN 範圍任何為數字）

>[!NOTE]
>不支援四個位元組 ASNs SLB 日 MUX。 您必須將有兩個位元組 ASNs 配置 SLB 日 MUX 和路由器 wo 的連接。 您可以針對您的環境中使用 4 位元組 ASNs。  
  
您或您的網路系統管理員必須設定接受來自 ASN 及 IP 位址或使用您的 RAS 閘道和 SLB 日 MUXes 轉送邏輯網路子網路位址 BGP 路由器等。
  
如需詳細資訊，請查看[邊境閘道通訊協定與 #40;BGP 和 #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).
  
## <a name="default-gateways"></a>預設閘道

設定為多個網路，例如實體主機閘道虛擬電腦連接的電腦必須只有一個預設閘道設定。 預設閘道通常會在連接到網際網路時所使用的介面卡設定。

虛擬的電腦，來選擇要使用做為預設閘道網路使用下列的規則：

1. 如果一樣已連接到轉送網路，或是否多重傳輸及任何其他網路做預設閘道轉送網路。  
2. 如果一樣只連接到管理網路，做預設閘道管理網路。  
3.  不得預設閘道用於 HNV 提供者網路。 僅限虛擬電腦已連接此網路將 SLB 日 MUXes 和 RAS 閘道。  
4.  虛擬電腦將不會直接連接到 Storage1、Storage2、公用 VIP 或 VIP 私人網路。  
  
HYPER-V 主機和儲存節點，做為預設閘道使用管理網路。  儲存空間的網路不會必須預設閘道指派。
  
## <a name="network-hardware"></a>網路上的硬體

規劃網路硬體部署，您可以使用下列的各節。

### <a name="network-interface-cards-nics"></a>網路介面卡 (Nic)

若要達到最佳效能，在您使用您的 HYPER-V 主機與儲存空間主機網路介面卡需要特定功能。  
 
遠端直接記憶體存取 (RDMA) 是核心略過技巧，可讓大量的資料傳輸不需要 CPU 主機。 DMA 引擎網路介面卡上的執行傳輸，因為 CPU 不會被用於記憶體移動的區域。  這樣會釋放 CPU 執行其他工作。  

切換 Embedded 小組（設定）是另一個方法 NIC 小組方案，您可以在 Windows Server 2016 中包含 HYPER-V 和軟體所定義網路 (SDN) 堆疊的環境中使用。 設定 HYPER-V Virtual 開關切換至整合 NIC 小組的某些功能。

如需詳細資訊，請查看[遠端直接記憶體存取和 #40;RDMA 與 #41;切換 Embedded 小組與 #40; 以及設定與 #41;](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

若要進行了造成 VXLAN 或 NVGRE 封裝標頭承租人 virtual 網路流量的負荷，MTU 的層級 2 fabric 網路（參數和主機）必須設為大於或等於 1674 位元組 \（包括層級 2 乙太網路 headers\）。 Nic 支援新的*EncapOverhead*關鍵字進階介面卡將會自動透過網路控制器主機代理程式 MTU。 Nic 並支援新的*EncapOverhead*需要在使用每個實體主機上手動設定 MTU 大小關鍵字*JumboPacket* \(or equivalent\) 關鍵字。

### <a name="switches"></a>切換
  
選取實體切換和路由器，您的環境，請確定它支援下列設定的功能。  

- Switchport MTU 設定 \(required\)  
- 設定 MTU > = 1674 位元組 \（包括 L2-乙太網路 Header\）  
- L3 通訊協定 \(required\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-based ECMP

實作應該支援下列 IETF 標準必須聲明。

- RFC 2545:「BGP-4 多重通訊協定擴充功能 IPv6 間網域路由」  
- RFC 4760:「多重通訊協定擴充功能 BGP-4]  
- RFC 4893:」為八四 BGP 支援號碼的空間]  
- RFC 4456:「BGP 路由反映：完整的替代方案 Mesh 內部 BGP (IBGP)」  
- RFC 4724:「優雅重新開機機制 BGP」  

下列標記通訊協定所需項目。

- VLAN-隔離各種不同類型的資料傳輸
- 802.1q 主幹

下列項目提供連結控制。

- 服務品質 \ (如果您使用 RoCE\，只需 PFC)
- 增強流量 \(802.1Qaz\) 選取項目
- 優先順序根據流量控制 \（802.1 p 日 Q 和 802.1Qbb\）

下列項目提供可用性和冗餘。

- 切換可用性（必要）
- 可用性路由器，才能執行閘道功能。 您可以使用多底座 switch\ 路由器或 VRRP 類似技術。
        
下列項目提供管理功能。

**監視**

- SNMP v1 或 SNMP v2（如果實體切換監視使用 Network Controller 必要）  
- SNMP Mib \（如果您使用 Network Controller 的實體切換 monitoring\ 必填）  
- MIB-II (RFC 1213)、LLDP，介面 MIB \(RFC 2863\)，如果-MIB，IP MIB、IP-轉寄-MIB、Q-橋接器-MIB、橋接器-MIB、LLDB-MIB、實體-MIB、IEEE8023-延遲-MIB  
  
下圖顯示範例四個節點設定。 目的清晰度、的第一個圖表顯示網路控制器、第二個會顯示加網路控制器上的軟體負載平衡器和第三個圖表顯示網路控制器、軟體負載平衡器，以及閘道。  
  
儲存空間網路和 vNICs 並不在這些圖表 shonwn。 如果您打算使用 smb 存放裝置，這些都是需要。    
  
（假設正確的網路連接存在正確邏輯網路）任何實體運算主機上進行轉散發基礎結構和承租人虛擬電腦。  
  
### <a name="network-controller-deployment"></a>網路控制器部署

部署 Network Controller 之前，您必須檢視安裝與軟體需求，以及設定安全性群組和動態 DNS 登記。 如需詳細資訊，請查看[安裝和準備需求部署 Network Controller 的](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)。

設定是高度提供三個 Network Controller 節點虛擬電腦上設定。 也會顯示為兩個 tenants 承租人 2 virtual 網路分為兩個 virtual 子網路模擬 web 層和資料庫層。  

![SDN NC 計劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-NC-Planning.png)

### <a name="network-controller-and-software-load-balancer-deployment"></a>網路控制器和軟體負載平衡器部署

可用性，有兩個或更多 SLB 日 MUX 節點。
   
![SDN NC 計劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-SLB-Deployment.png)
  
### <a name="network-controller-software-load-balancer-and-ras-gateway-deployment"></a>Network Controller、軟體負載平衡器，以及 RAS 閘道部署

有三種閘道虛擬電腦。有兩種是使用中狀態，並有重複。

![SDN NC 計劃](../../media/Plan-a-Software-Defined-Network-Infrastructure/SDN-GW-Deployment.png)  
  
  
  
TP5 為基礎的部署自動化、Active Directory 必須中提供，且可以從這些子網路。 如需有關 Active Directory 的詳細資訊，請查看[Active Directory Domain Services 概觀](https://technet.microsoft.com/en-us/library/mt703721.aspx)。  
  
>[!IMPORTANT] 
>如果您使用 VMM 部署，確保您的基礎結構虛擬電腦 (VMM Server 廣告日 DNS，SQL Server 等) 無法裝載任何圖表中顯示的四個主機上。  
  
## <a name="switch-configuration-examples"></a>切換設定範例  
  
若要設定您的路由器或實體切換，會有各種不同的模式開關切換至和廠商範例設定檔的一組[Microsoft SDN Github 存放庫](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples)。 提供詳細讀我及測試的命令列介面 (CLI) 參數特定的命令。         
  
  
## <a name="compute"></a>計算  
所有 HYPER-V 主機必須都已安裝 Windows Server 2016、HYPER-V 功能，並建立一個以上的實體介面卡的外部 HYPER-V virtual 切換連接管理邏輯網路。 主機必須可透過指派給管理主機但 vNIC 管理 IP 位址。  
  
可使用任何相容於 HYPER-V，共用或本機存放裝置類型。   
  
> [!TIP]  
> 如果您使用的相同名稱的所有 virtual 參數，但是並不一定，是便利。 如果您要部署的指令碼，查看意見相關的`vSwitchName`變數 config.psd1 檔案中。  
  
**主機運算需求**  
如下表範例部署中使用的四個實體主機上的硬體和軟體的最低需求。  
  
主機|硬體需求|軟體需求|  
--------|-------------------------|-------------------------  
|實體 hyper-v 主機|4 核心 2.66 GHz CPU<br /><br />32 GB 的 RAM<br /><br />300 GB 磁碟空間<br /><br />1 Gb/秒（或更快）實體網路介面卡|作業系統：Windows Server 2016<br /><br />安裝 HYPER-V 角色|  
  
  
**SDN 基礎結構一樣角色需求**  
  
角色|vCPU 需求|記憶體需求|磁碟需求|  
--------|---------------------|-----------------------|---------------------  
|網路控制器（三個節點）|4 vCPUs|4 GB 分鐘 (建議 8 GB)|作業系統的磁碟機 75 GB  
|SLB MUX（三個節點）|8 vCPUs|建議 8 GB|作業系統的磁碟機 75 GB  
|RAS 閘道<br /><br />（單一集區的三個節點閘道、兩個主動式一個被動式）|8 vCPUs|建議 8 GB|作業系統的磁碟機 75 GB  
|適用於對等 SLB 日 MUX RAS 閘道 BGP 路由器<br /><br />（或者使用 ToR 切換為 BGP 路由器）|2 vCPUs|使用 2 GB|作業系統的磁碟機 75 GB|  
  
  
如果您使用 VMM 部署，其他的基礎結構一樣的資源將會需要 VMM 和其他非 SDN 基礎結構。 如需詳細資訊，請查看[適用於系統中心 Technical Preview 最小值硬體建議。](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>延伸您的基礎結構  
您的基礎結構的大小和資源需求的相關承租人工作負載虛擬機器想要主機上。 CPU、記憶體及需求磁碟的基礎結構虛擬電腦 (例如：網路控制器，請 SLB，閘道等) 上一個表格中所列。 您可以新增多個這些分攤為所需的基礎結構虛擬電腦。 不過，HYPER-V 主機上執行的任何承租人虛擬電腦都有自己的 CPU、記憶體及，您必須考慮磁碟需求。   
  
當承租人工作負載虛擬機器開始使用太多資源實體 HYPER-V 主機時，您可以透過新增額外的實體主機延伸您的基礎結構。 這可以使用一樣管理員或使用 PowerShell 指令碼（根據您一開始部署方式基礎結構）來建立新的伺服器資源透過網路控制器。 如果您需要新增額外的 HNV 提供者網路的 IP 位址，您可以建立新邏輯子網路（以對應 IP 集區中），可以使用主機。  
  
  
## <a name="see-also"></a>也了  
[安裝和部署 Network Controller 準備需求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[軟體定義網路與 #40;SDN 與 #41;](../Software-Defined-Networking--SDN-.md)  
  


