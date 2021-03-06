---
title: 規劃軟體定義網路的基礎結構
description: 本主題提供如何規劃軟體定義網路 (SDN) 基礎結構部署的相關資訊。
manager: grcusanz
ms.topic: how-to
ms.assetid: ea7e53c8-11ec-410b-b287-897c7aaafb13
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/10/2018
ms.openlocfilehash: 0473b130b53018132ff5e705808a6fd3e37fccaa
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716494"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>規劃軟體定義網路的基礎結構

>適用於：Windows Server 2019、Windows Server 2016

深入瞭解軟體定義網路基礎結構的部署規劃，包括硬體和軟體必要條件。

## <a name="prerequisites"></a>Prerequisites
本主題說明一些硬體和軟體必要條件，包括：

-   **已設定安全性群組、記錄檔位置和動態 DNS 註冊。** 您必須準備資料中心以進行網路控制站部署，這需要一或多部電腦或 vm，以及一部電腦或 VM。 部署網路控制站之前，您必須先設定安全性群組、記錄檔位置 (（如有需要）) 和動態 DNS 註冊。  如果您尚未準備資料中心進行網路控制站部署，請參閱 [部署網路控制站的安裝和準備需求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) 以取得詳細資料。

-   **實體網路。**  您需要存取您的實體網路裝置，以便設定 Vlan、路由、BGP、資料中心橋接 (ETS) 如果使用 RDMA 技術，而且資料中心橋接 (PFC) 如果使用 RoCE 型 RDMA 技術。 本主題說明在第3層交換器/路由器上的手動交換器設定以及 BGP 對等互連，或 (RRAS) 虛擬機器的路由和遠端存取服務器。

-   **實體計算主機。**  這些主機會執行 Hyper-v，而且必須裝載 SDN 基礎結構和租使用者虛擬機器。  這些主機需要特定的網路硬體，以獲得最佳效能，稍後會在 [ **網路硬體** ] 區段中加以說明。


## <a name="physical-network-and-compute-host-configuration"></a>實體網路和計算主機設定

每個實體計算主機都需要透過一或多個連接到實體交換器埠 (s 的網路介面卡進行網路連線) 。  第2層 [VLAN](https://en.wikipedia.org/wiki/Virtual_LAN) 支援分割成多個邏輯網路區段的網路。

>[!TIP]
>存取模式中的邏輯網路使用 VLAN 0 或未標記。

>[!IMPORTANT]
>Windows Server 2016 軟體定義的網路功能支援基礎和覆迭的 IPv4 定址。 不支援 IPv6。

### <a name="logical-networks"></a>Logical networks

#### <a name="management-and-hnv-provider"></a>管理與 HNV 提供者

所有實體計算主機都必須存取管理邏輯網路和 HNV 提供者邏輯網路。  針對 IP 位址規劃目的，每個實體計算主機至少必須有一個從管理邏輯網路指派的 IP 位址。 網路控制站需要保留的 IP 位址，才能作為 REST IP 位址。

DHCP 伺服器可以自動指派管理網路的 IP 位址，您也可以手動指派靜態 IP 位址。 SDN 堆疊會自動指派 IP 位址給個別 Hyper-v 主機的 HNV 提供者邏輯網路，從指定的 IP 集區，以及由網路控制站管理的 IP 集區。

>[!NOTE]
>網路控制站只會在網路控制站主機代理程式收到特定租使用者虛擬機器的網路原則之後，才將 HNV 提供者 IP 位址指派給實體計算主機。


|                                                               如果...                                                               |                                                                                                                                                                          則...                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                  邏輯網路使用 Vlan，                                                  |                                                                 實體計算主機必須連接到可存取這些 Vlan 的主幹交換器埠。 請務必注意，電腦主機上的實體網路介面卡不能啟用任何 VLAN 篩選。                                                                 |
|                使用 Switched-Embedded 小組 (設定) ，並讓多個 NIC 小組成員（例如網路介面卡）                |                                                                                                                        您必須將該特定主機的所有 NIC 小組成員連線至相同的第2層廣播網域。                                                                                                                         |
| 實體計算主機正在執行額外的基礎結構虛擬機器，例如網路控制站、SLB/MUX 或閘道。 | 該主機必須針對每個裝載的虛擬機器，從管理邏輯網路指派一個額外的 IP 位址。<p>此外，每個 SLB/MUX 基礎結構虛擬機器都必須有保留給 HNV 提供者邏輯網路的 IP 位址。 若無法保留 IP 位址，可能會導致您的網路上有重複的 IP 位址。 |

---

如需 Hyper-v 網路虛擬化 (HNV) 的相關資訊，您可以使用它來虛擬化 Microsoft SDN 部署中的網路，請參閱 [Hyper-v 網路虛擬化](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)。



#### <a name="gateways-and-the-software-load-balancer"></a>閘道和軟體 Load Balancer

您必須建立並布建額外的邏輯網路，以取得閘道和 SLB 的使用方式。 請務必取得這些網路的正確 IP 首碼、VLAN Id 和閘道 IP 位址。


|                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **傳輸邏輯網路**   | RAS 閘道和 SLB/MUX 使用傳輸邏輯網路來交換 BGP 對等互連資訊，而北/南部 (外部內部) 租使用者流量。 此子網的大小通常會小於其他子網的大小。 只有執行 RAS 閘道或 SLB/MUX 虛擬機器的實體計算主機需要能夠連線到此子網，這些 Vlan 會主幹並可在計算主機網路介面卡所連線的交換器埠上存取。 每個 SLB/MUX 或 RAS 閘道虛擬機器都會從傳輸邏輯網路以靜態方式指派一個 IP 位址。 |
| **公用 VIP 邏輯網路**  |                                                                                                                             公用 VIP 邏輯網路必須有可在雲端環境外路由傳送的 IP 子網首碼 (通常是網際網路可路由傳送的) 。  這些將是外部用戶端用來存取虛擬網路中資源的前端 IP 位址，包括站對站閘道的前端 VIP。                                                                                                                             |
| **私人 VIP 邏輯網路** |                                                                                                                                                                                       私人 VIP 邏輯網路不需要在雲端外路由傳送，因為它是用於僅可從內部雲端用戶端（例如 SLB 管理員或私用服務）存取的 Vip。                                                                                                                                                                                       |
|   **GRE VIP 邏輯網路**   |                                                                                                                                           GRE VIP 網路是一個子網路，僅為了針對 S2S GRE 連線類型，定義指派給 SDN 網狀架構上執行之閘道虛擬機器的 VIP 而存在。 此網路在實體交換器或路由器中不需要預先設定，而且不需要指派 VLAN。                                                                                                                                            |

---


#### <a name="sample-network-topology"></a>網路拓撲範例
變更您環境的範例 IP 子網首碼和 VLAN 識別碼。


| **網路名稱** |  **子網路**  | **Mask** | **貨車上的 VLAN 識別碼** | **閘道**  |                                                           **保留 (範例)**                                                           |
|------------------|--------------|----------|----------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
|    管理性    | 10.184.108.0 |    24    |          7           | 10.184.108.1 | 10.184.108.1 –路由器 10.184.108.4-網路控制器 10.184.108.10-計算主機 110.184.108.11-計算主機 210.184.108. X-計算主機 X |
|   HNV 提供者   |  10.10.56.0  |    23    |          11          |  10.10.56.1  |                                                    10.10.56.1 –路由器 10.10.56.2-SLB/MUX1                                                     |
|     交通      |  10.10.10.0  |    24    |          10          |  10.10.10.1  |                                                               10.10.10.1 –路由器                                                               |
|    公用 VIP    |  41.40.40.0  |    27    |          NA          |  41.40.40.1  |                                    41.40.40.1 –路由器 41.40.40.2-SLB/MUX VIP 41.40.40.3-IPSec S2S VPN VIP                                    |
|   私密 VIP    |  20.20.20.0  |    27    |          NA          |  20.20.20.1  |                                                        20.20.20.1-預設 GW (路由器)                                                          |
|     GRE VIP      |  31.30.30.0  |    24    |          NA          |  31.30.30.1  |                                                             31.30.30.1-預設 GW                                                             |

---

### <a name="logical-networks-required-for-rdma-based-storage"></a>以 RDMA 為基礎的儲存體所需的邏輯網路

如果使用 RDMA 型存放裝置，請針對每個實體介面卡定義 VLAN 和子網 (在您的計算和儲存體主機中，) 每個節點各有兩張介面卡。

>[!IMPORTANT]
>若要適當地套用服務品質 (QoS) ，實體交換器需要已標記的 VLAN 來進行 RDMA 流量。

| **網路名稱** |  **子網路**  | **Mask** | **貨車上的 VLAN 識別碼** | **閘道**  |                                                           **保留 (範例)**                                                            |
|------------------|--------------|----------|----------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
|     儲存體1     |  10.60.36.0  |    25    |          8           |  10.60.36.1  |  10.60.36.1 –路由器<p>10.60.36 x-計算主機 X<p>10.60.36 y-計算主機 Y<p>10.60.36. V-計算叢集<p>10.60.36 （W）-儲存體叢集  |
|     >storage2     | 10.60.36.128 |    25    |          9           | 10.60.36.129 | 10.60.36.129 –路由器<p>10.60.36 x-計算主機 X<p>10.60.36 y-計算主機 Y<p>10.60.36. V-計算叢集<p>10.60.36 （W）-儲存體叢集 |

---


## <a name="routing-infrastructure"></a>路由基礎結構

如果您要使用腳本來部署 SDN 基礎結構，則必須在實體網路上互相路由傳送管理、HNV 提供者、傳輸和 VIP 子網。

\( \) 使用內部 BGP 對等互連，在實體網路中將 SLB/MUX 和 RAS 閘道通告的路由資訊（例如，VIP 子網的下一個躍點）。 VIP 邏輯網路沒有指派 VLAN，且未在第2層交換器中預先設定 (例如，機架上切換) 。

您必須在您的 SDN 基礎結構所用的路由器上建立 BGP 對等互連，以接收 SLB/Mux 和 RAS 閘道所公告的 VIP 邏輯網路路由。 BGP 對等互連只需要從 SLB/MUX 或 RAS 閘道 (到外部 BGP 對等) 。  不過，在第一層的路由中，您可以使用靜態路由或其他動態路由通訊協定（例如，如先前所述），VIP 邏輯網路的 IP 子網首碼必須可從實體網路路由傳送至外部 BGP 對等。

BGP 對等互連通常是在受管理交換器或路由器中設定為網路基礎結構的一部分。 您也可以在具有遠端存取服務器的 Windows 伺服器上設定 BGP 對等， (RAS) 角色安裝在僅限路由模式中。 網路基礎結構中的此 BGP 路由器對等必須設定為具有自己的 ASN，並允許從指派給 SDN 元件 \( SLB/MUX 和 RAS 閘道的 asn 進行對等互連 \) 。 您必須從您的實體路由器或從該路由器控制的網路系統管理員取得下列資訊：

- 路由器 ASN
- 路由器 IP 位址
- 用於 SDN 元件的 ASN (可以是私人 ASN 範圍中的任何 AS 編號) 

>[!NOTE]
>SLB/MUX 不支援四個位元組的 Asn。 您必須將兩個位元組 Asn 配置給 SLB/MUX，以及它所連接的路由器 wo。 您可以在環境中的其他地方使用4個位元組的 Asn。

您或您的網路系統管理員必須設定 BGP 路由器對等，以接受來自您 RAS 閘道和 SLB/Mux 所使用之傳輸邏輯網路的 ASN 和 IP 位址或子網位址的連線。

如需詳細資訊，請參閱 [邊界閘道協定 (BGP) ](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)。

## <a name="default-gateways"></a>預設閘道
設定為連線至多個網路的電腦，例如實體主機和閘道虛擬機器，必須設定一個預設閘道。 在用來連接網際網路的介面卡上設定預設閘道。

針對虛擬機器，請遵循這些規則來決定要使用哪個網路作為預設閘道：

1. 如果虛擬機器連線到傳輸網路，或者如果它是多路連接到傳輸網路或任何其他網路，請使用傳輸邏輯網路作為預設閘道。
2. 如果虛擬機器只連接到管理網路，請使用管理網路作為預設閘道。
3. 使用適用于 SLB/Mux 和 RAS 閘道的 HNV 提供者網路。 請勿使用 HNV 提供者網路作為預設閘道。
4. 請勿將虛擬機器直接連線至 Storage1、>storage2、公用 VIP 或私人 VIP 網路。

若為 Hyper-v 主機和存放裝置節點，請使用管理網路作為預設閘道。  存放裝置網路絕不能指派預設閘道。


## <a name="network-hardware"></a>網路硬體

您可以使用以下各節來規劃網路硬體部署。

### <a name="network-interface-cards-nics"></a>網路介面卡 (NIC)

Hyper-v 主機和存放裝置主機中使用的網路介面卡 (Nic) 需要特定功能才能達到最佳效能。

遠端直接記憶體存取 (RDMA) 是一種核心略過技術，可讓您在不使用主機 CPU 的情況下傳輸大量資料，進而釋出 CPU 來執行其他工作。

交換器 Embedded 小組 (設定) 是替代的 NIC 小組解決方案，可供您在 Windows Server 2016 中包含 Hyper-v 和軟體定義網路 (SDN) 堆疊的環境中使用。 SET 可將某些 NIC 小組功能整合到 Hyper-v 虛擬交換器。

如需詳細資訊，請參閱 [ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

為了考慮 VXLAN 或 NVGRE 封裝標頭所造成的租使用者虛擬網路流量的額外負荷， (交換器和主機) 的第2層網狀架構網路的 MTU 必須設定為大於或等於1674個位元組， \( 包括第2層乙太網路標頭 \) 。

支援 new *EncapOverhead* advanced adapter 關鍵字的 nic 會透過網路控制卡主機代理程式自動設定 MTU。 不支援 new *EncapOverhead* 關鍵字的 nic 必須在每個實體主機上使用 *JumboPacket* \( 或對等關鍵字手動設定 MTU 大小 \) 。


### <a name="switches"></a>交換器

為您的環境選取實體交換器和路由器時，請確定它支援下列功能集：

- 需要的設定埠 MTU 設定 \(\)
- 將 MTU 設定為 >= 1674 個位元組， \( 包括 L2-Ethernet 標頭\)
- 需要 L3 通訊協定 \(\)
- ECMP
- BGP \( IETF RFC 4271 型 \) \- ECMP

執行應支援下列 IETF 標準中的「必須」語句。

- RFC 2545：「IPv6 Inter-Domain 路由的 BGP-4 多重通訊協定延伸」
- RFC 4760：「BGP-4 的多重通訊協定延伸」
- RFC 4893：「BGP 支援四個八位的數位空格」
- RFC 4456：「BGP 路由反映：完整網狀內部 BGP (IBGP) 」的替代方案
- RFC 4724：「適用于 BGP 的正常重新開機機制」

需要下列標記通訊協定。

- VLAN-隔離各種類型的流量
- 802.1 q 主幹

下列專案提供連結控制。

- \(只有在使用 RoCE 時才需要服務品質 PFC\)
- 增強的流量選取 \( 802.1 qaz\)
- 以優先順序為基礎 \( 的流量控制 802.1 p/Q 和 802.1 qbb\)

下列專案提供可用性和冗余。

- 切換可用性 (所需) 
- 需要高可用性的路由器，才能執行閘道功能。 您可以使用多底座交換器 \ 路由器或 VRRP 等技術來達成此目的。

下列專案提供管理功能。

**監視**

- 如果使用網路控制站進行實體交換器監視，則需要 SNMP v1 或 SNMP v2 () 
- \(如果您使用網路控制站進行實體交換器監視，則需要 SNMP mib\)
- MIB-II (RFC 1213) 、LLDP、Interface MIB \( RFC 2863 \) 、IF-MIB、IP-HTTPS、ip 轉送-Mib、Q-BRIDGE-MIB、LLDB、IEEE8023-LAG、ENTITY-MIB、-mib

下圖顯示四個節點的設定範例。 為了清楚起見，第一個圖表只會顯示網路控制站，第二個則顯示網路控制卡加上軟體負載平衡器，而第三個圖表則顯示網路控制站、軟體負載平衡器和閘道。

這些圖表不會顯示儲存體網路和 Vnic。 如果您打算使用 SMB 儲存空間，則需要這些。

基礎結構和租使用者虛擬機器都可以在任何實體計算主機之間重新分配 (假設正確的邏輯網路) 有正確的網路連線能力。



## <a name="switch-configuration-examples"></a>交換器設定範例

為協助設定您的實體交換器或路由器， [MICROSOFT SDN GitHub 存放庫](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples)中有一組適用于各種交換器模型和廠商的範例設定檔。 提供詳細的讀我檔案和測試的命令列介面 (CLI) 特定參數的命令。


## <a name="compute"></a>計算
所有 Hyper-v 主機都必須安裝 Windows Server 2016、啟用 Hyper-v，以及使用至少一個連接到管理邏輯網路的實體介面卡建立的外部 Hyper-v 虛擬交換器。 主機必須可透過指派給管理主機 vNIC 的管理 IP 位址來連線。

您可以使用任何與 Hyper-v、共用或本機相容的儲存體類型。

> [!TIP]
> 如果您的所有虛擬交換器都使用相同的名稱，則會很方便，但它並不是必要的。 如果您打算使用腳本進行部署，請參閱 config.psd1 檔案中與變數相關聯的批註 `vSwitchName` 。

**主機計算需求** 下表顯示範例部署中使用之四個實體主機的最低硬體和軟體需求。

主機|硬體需求|軟體需求|
--------|-------------------------|-------------------------
|實體 Hyper-v 主機|四核心 2.66 GHz CPU<p>32 GB 的 RAM<p>300 GB 磁碟空間<p>1 Gb/秒 (或更快的) 實體網路介面卡|作業系統： Windows Server 2016<p>已安裝 hyper-v 角色|


**SDN 基礎結構虛擬機器角色需求**

角色|vCPU 需求|記憶體需求|磁碟需求|
--------|---------------------|-----------------------|---------------------
|網路控制站 (三個節點) |4個 vcpu| (8 GB 的建議) 4 GB 分鐘|適用于 OS 磁片磁碟機 75 GB
|SLB/MUX (三個節點) |8 個 vCPU|建議 8 GB|適用于 OS 磁片磁碟機 75 GB
|RAS 閘道<p> (三個節點閘道的單一集區，兩個主動、一個被動) |8 個 vCPU|建議 8 GB|適用于 OS 磁片磁碟機 75 GB
|適用于 SLB/MUX 對等互連的 RAS 閘道 BGP 路由器<p> (或者使用 ToR 交換器作為 BGP 路由器) |2個 vcpu|2 GB|適用于 OS 磁片磁碟機 75 GB|


如果您使用 VMM 進行部署，則 VMM 和其他非 SDN 基礎結構需要額外的基礎結構虛擬機器資源。 如需詳細資訊，請參閱 [System Center Technical Preview 的最低硬體建議。](/system-center/)

## <a name="extending-your-infrastructure"></a>擴充您的基礎結構
基礎結構的大小和資源需求取決於您打算裝載的租使用者工作負載虛擬機器。 基礎結構虛擬機器的 CPU、記憶體和磁片需求 (例如：網路控制站、SLB、閘道等 ) 列于上表中。 您可以視需要新增更多基礎結構虛擬機器以相應放大。 不過，在 Hyper-v 主機上執行的任何租使用者虛擬機器都有自己的 CPU、記憶體和磁片需求，您必須考慮這些虛擬機器。

當租使用者工作負載虛擬機器在實體 Hyper-v 主機上開始耗用太多資源時，您可以藉由新增額外的實體主機來擴充您的基礎結構。 這可以透過 Virtual Machine Manager 或使用 PowerShell 腳本 (來完成，這取決於您最初部署基礎結構) 如何透過網路控制站來建立新的伺服器資源。 如果您需要為 HNV 提供者網路新增其他 IP 位址，您可以建立新的邏輯子網， (具有主機可使用的對應 IP 集區) 。


## <a name="see-also"></a>另請參閱
[部署網路控制](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md) 
 站的安裝與準備需求[軟體定義的網路 &#40;SDN&#41;](../software-defined-networking.md)