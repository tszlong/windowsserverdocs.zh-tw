---
title: 規劃軟體定義網路的基礎結構
description: 本主題提供有關如何規劃軟體定義網路 (SDN) 基礎結構部署的資訊。
manager: dougkim
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
ms.date: 08/10/2018
ms.openlocfilehash: e3f7f2b6e2f815ec1924b4d476d5e3fd3ab181ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821059"
---
# <a name="plan-a-software-defined-network-infrastructure"></a>規劃軟體定義網路的基礎結構

>適用於：Windows Server （半年通道），Windows Server 2016

深入了解部署規劃軟體定義網路基礎結構，包括硬體和軟體必要條件。 


## <a name="prerequisites"></a>先決條件
本主題描述幾個的硬體和軟體必要條件，包括：

-   **設定安全性群組、 記錄檔位置，以及動態 DNS 註冊**您必須準備您的資料中心網路控制站部署，而這需要一個或多部電腦或 Vm 及一台電腦或 VM。 您可以部署網路控制站之前，您必須設定安全性群組、 記錄檔位置 （如有需要），以及動態 DNS 註冊。  如果未備妥您的資料中心網路控制卡部署，請參閱[安裝和部署網路控制站準備需求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)如需詳細資訊。

-   **實體網路**您需要存取您實體網路裝置，以便設定 Vlan、 路由，BGP，資料中心橋接 (ETS) 如果使用 RDMA 技術，以及資料中心橋接 (PFC) 如果使用 RoCE 基礎 RDMA 技術。 本主題說明手動切換組態以及 BGP 對等互連上第 3 層交換器 / 路由器或路由及遠端存取伺服器 (RRAS) 的虛擬機器。   

-   **實體計算主機**這些主機執行 HYPER-V，以及所需裝載 SDN 基礎結構和租用戶虛擬機器。  為了達到最佳效能，將在稍後會說明這些主機中需要特定的網路硬體**網路硬體**一節。  
      
  
## <a name="physical-network-and-compute-host-configuration"></a>實體網路和運算的主應用程式組態

每個實體計算主機需要透過附加至實體交換器連接埠的一或多個網路介面卡的網路連線。  2 層[VLAN](https://en.wikipedia.org/wiki/Virtual_LAN)支援網路分成多個邏輯網路區段。 

>[!TIP]
>用於存取模式中的邏輯網路的 VLAN 0 或未標記。 

>[!IMPORTANT]
>Windows Server 2016 軟體定義網路功能支援 IPv4 定址為和覆疊。 IPv6 不受支援。
  
### <a name="logical-networks"></a>邏輯網路

#### <a name="management-and-hnv-provider"></a>管理和 HNV 提供者 

所有實體計算主機必須存取的管理邏輯網路和 HNV 提供者邏輯網路。  規劃用途的 IP 位址，每個實體計算主機必須至少一個從管理邏輯網路指派的 IP 位址。 網路控制站需要保留的 IP 位址，做為 REST IP 位址。 

DHCP 伺服器可以自動指派 IP 位址管理網路，或者您可以手動指派靜態 IP 位址。 SDN 堆疊會自動指派 IP 位址的 HNV 提供者透過指定 IP 集區中個別的 HYPER-V 主機的邏輯網路，和管理網路控制站。 

>[!NOTE]
>網路控制站會將 HNV 提供者 IP 位址指派給實體計算主機中，才在網路控制站主機代理程式收到針對特定租用戶虛擬機器的網路原則。 


|如果...  |則...  |
|---------|---------|
|邏輯網路是使用 Vlan，     |實體計算主應用程式必須連接到可存取這些 Vlan 主幹的交換器連接埠。 請務必請注意，電腦主機上的實體網路介面卡必須沒有啟動任何 VLAN 篩選。          |
|使用 Switched-Embedded Teaming (SET)，且有多個 NIC 小組成員，例如網路介面卡，     |您必須連接至相同的第 2 層廣播網域該特定主控件的 NIC 小組成員的所有。         |
|實體計算主機正在執行額外的基礎結構虛擬機器，例如網路控制卡、 SLB/MUX 或閘道，  |該主機必須有額外的 IP 位址從管理邏輯網路指派給每個裝載的虛擬機器。<p>此外，每個 SLB/MUX 基礎結構虛擬機器必須具有保留的 HNV 提供者邏輯網路的 IP 位址。 沒有保留的 IP 位址可能會導致重複的 IP 位址，在您網路上。  |
---

如需有關 HYPER-V 網路虛擬化 (HNV)，您可以使用虛擬化 Microsoft SDN 部署中的網路，資訊[HYPER-V 網路虛擬化](../technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)。  
  

  
#### <a name="gateways-and-the-software-load-balancer"></a>閘道和軟體負載平衡器
  
其他的邏輯網路，就需要建立及佈建的閘道和 SLB 使用方式。 請務必取得正確的 IP 首碼、 VLAN Id，以及這些網路的閘道 IP 位址。


|  |  |
|---------|---------|
|**傳輸邏輯網路**     |RAS 閘道和 SLB/MUX 使用傳輸邏輯網路交換 BGP 對等互連的資訊和北/南 （外部-內部） 的租用戶流量。 這個子網路的大小通常會比其他較小。 只有實體計算執行 RAS 閘道或 SLB/MUX 虛擬機器的主機需要在計算主機的網路介面卡所連接的交換器連接埠上連線到這些 vlan 主幹且可存取此子網路。 每個 SLB/MUX 或 RAS 閘道虛擬機器以靜態方式指派一個 IP 位址從傳輸邏輯網路。         |
|**公用 VIP 邏輯網路**     |公用 VIP 邏輯網路，才能有雲端環境 （通常是網際網路可路由傳送） 外部路由傳送的 IP 子網路首碼。  這些會是外部用戶端用來存取資源，包括站對站閘道的前端 VIP 的虛擬網路中的前端 IP 位址。         |
|**私人 VIP 邏輯網路**     |不是在雲端外部路由傳送，因為只會從內部雲端的用戶端，例如 SLB Mananger 或私用的服務存取的 vip 將會用需要私人 VIP 邏輯網路。         |
|**GRE VIP 邏輯網路**     |GRE VIP 網路是僅供定義 Vip 以指派給在 SDN 網狀架構 S2S GRE 連線類型上執行的閘道虛擬機器存在的子網路。 此網路不需要在您的實體交換器或路由器中預先設定並不需要指派 VLAN。            |
---


#### <a name="sample-network-topology"></a>網路拓樸範例
為您的環境中變更的範例 IP 子網路前置詞和 VLAN Id。 

| **網路名稱** | **Subnet** | **遮罩** | **在 「 卡車 」 上的 VLAN ID** | **Gateway** | **保留項目 （範例）** |
| --- | --- | --- | --- | --- | --- |
| Management | 10.184.108.0 | 24 | 7 | 10.184.108.1 | 10.184.108.1 – Router10.184.108.4-網路 Controller10.184.108.10-計算主機 110.184.108.11-計算裝載 210.184.108.X-計算主機 X |
| HNV 提供者 | 10.10.56.0 | 23 | 11 | 10.10.56.1 | 10.10.56.1 – Router10.10.56.2 - SLB/MUX1   |
| 傳輸 | 10.10.10.0 | 24 | 10 | 10.10.10.1 | 10.10.10.1-路由器 |
| 公用 VIP | 41.40.40.0 | 27 | NA | 41.40.40.1 | 41.40.40.1 – Router41.40.40.2 SLB/MUX VIP41.40.40.3-IPSec S2S VPN VIP   |
| 私人 VIP | 20.20.20.0 | 27 | NA | 20.20.20.1 | 20.20.20.1-預設 GW （路由器）   |
| GRE VIP | 31.30.30.0 | 24 | NA | 31.30.30.1 | 31.30.30.1-預設 GW |
---
  
### <a name="logical-networks-required-for-rdma-based-storage"></a>RDMA 為基礎的儲存體所需的邏輯網路  
  
如果使用 RDMA 型存放裝置，定義計算和儲存體主機中的 VLAN 和子網路，每個實體介面卡 （每個節點的兩個介面卡）。  

>[!IMPORTANT]
>實體交換器服務品質 (QoS)，適當地套用，需要 RDMA 流量已加上標籤的 VLAN。

| **網路名稱** | **Subnet** | **遮罩** | **在 「 卡車 」 上的 VLAN ID** | **Gateway** | **保留項目 （範例）** |
| --- | --- | --- | --- | --- | --- |
| storage1 | 10.60.36.0 | 25 | 8 | 10.60.36.1 | 10.60.36.1-路由器<p>10.60.36.X-計算主機 X<p>10.60.36.Y-計算主機 Y<p>10.60.36.V-計算叢集<p>10.60.36.W-存放裝置叢集 |
| storage2 | 10.60.36.128 | 25 | 9 | 10.60.36.129 | 10.60.36.129-路由器<p>10.60.36.X-計算主機 X<p>10.60.36.Y-計算主機 Y<p>10.60.36.V-計算叢集<p>10.60.36.W-存放裝置叢集   |
---

 
## <a name="routing-infrastructure"></a>路由基礎結構  
  
如果您要部署 SDN 基礎結構使用指令碼，管理 HNV 提供者、 傳輸，而且 VIP 子網路必須可路由傳送彼此在實體網路上。     
  
路由資訊\(例如下個躍點\)vip 子網路 SLB/MUX 和 RAS 閘道所公告至實體網路使用內部 BGP 對等互連。 VIP 邏輯網路不需要指派 VLAN，而且不會在第 2 層交換器 （例如機架頂端的交換器） 中預先設定。  
  
您需要建立可由 SDN 基礎結構來接收路由公告 SLB/Mux 和 RAS 閘道的 VIP 邏輯網路的路由器上的 BGP 對等。 BGP 對等互連只需要進行一種方式 （從 SLB/MUX 或外部的 BGP 對等的 RAS 閘道）。  上述第一層的路由，您可以使用靜態路由或另一個動態路由通訊協定例如 OSPF，不過，如先前所述，VIP 邏輯網路的 IP 子網路首碼需要可路由傳送至外部的 BGP 對等實體網路。   
  
中的受管理的交換器或路由器的網路基礎結構的一部分，通常設定 BGP 對等互連。 也無法使用遠端存取伺服器 (RAS) 角色路由僅模式安裝 Windows Server 上設定 BGP 對等。 這個的 BGP 路由器對等網路基礎結構中必須設定為有自己的 ASN，並允許 ASN 指派給 SDN 元件從對等互連\(SLB/MUX 和 RAS 閘道\)。 從實體的路由器或網路系統管理員，該路由器的控制項中，您必須取得下列資訊：

- 路由器 ASN  
- 路由器 IP 位址  
- （可以是私人 ASN 範圍從任何 AS 號碼） 的 SDN 元件所使用的 ASN

>[!NOTE]
>SLB/MUX 不支援四個位元組的 Asn。 您必須將兩個位元組的 Asn 配置給 SLB/MUX 和路由器 wo 它連接。 您可以在環境中的其他地方使用 4 個位元組的 Asn。  
  
您或您的網路系統管理員必須設定 BGP 路由器對等，以接受來自 ASN 和 IP 位址或子網路位址，您的 RAS 閘道和 SLB/Mux 使用傳輸邏輯網路的連線。
  
如需詳細資訊，請參閱 <<c0> [ 邊界閘道通訊協定 (BGP)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)。
  
## <a name="default-gateways"></a>預設閘道
設定為連線到多個網路，例如閘道虛擬機器與實體主機的機器只能有一個設定的預設閘道。 用來連線到網際網路的介面卡上設定的預設閘道。

針對虛擬機器，請遵循這些規則，以決定哪個網路將作為預設閘道：

1. 如果虛擬機器連接到轉送網路，或如果是多重主目錄，以傳輸網路或任何其他網路，請使用傳輸邏輯網路作為預設閘道。
2. 如果虛擬機器只會連接至管理網路，請使用與預設閘道的管理網路。 
3. 使用 HNV 提供者網路，SLB/Mux 和 RAS 閘道。 請勿使用 HNV 提供者網路的預設閘道。 
4. 不虛擬機器直接連接到 Storage1、 Storage2、 公用 VIP 或私人 VIP 網路。

如需 HYPER-V 主機和存放裝置節點，使用 管理網路作為預設閘道。  儲存體網路必須永遠不會被指派預設閘道。

  
## <a name="network-hardware"></a>網路硬體

您可以使用下列各節來規劃網路硬體部署。

### <a name="network-interface-cards-nics"></a>網路介面卡 (NIC)

使用您的 HYPER-V 主機和存放裝置主機網路介面卡 (Nic) 需要特定的功能，以達到最佳效能。 

遠端直接記憶體存取 (RDMA) 是核心旁路技術，讓您能夠傳輸大量資料，而不使用主機 CPU，釋出 CPU 來執行其他工作。 

Switch Embedded Teaming (SET) 是您可以使用包含 Windows Server 2016 中的 HYPER-V 和軟體定義網路 (SDN) 堆疊的環境中的 NIC 小組解決方案的替代方案。 設定會將部分 NIC 小組功能整合到 HYPER-V 虛擬交換器。 

如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](../../../virtualization//hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。   

為了說明 VXLAN 或 NVGRE 封裝標頭所造成的租用戶虛擬網路流量的額外負荷，第 2 層網狀架構網路 （交換器和主機） 的 MTU 必須設定為大於或等於 1674 位元組\(包括第 2 層乙太網路標頭\)。 

支援新的 Nic *EncapOverhead*進階配接器的關鍵字設定的 MTU，自動透過網路控制卡主機代理程式。 不支援新的 Nic *EncapOverhead*關鍵字必須使用每個實體主機上手動設定的 MTU 大小*JumboPacket* \(或同等權限\)關鍵字。 


### <a name="switches"></a>參數
  
選取的實體交換器和路由器，為您的環境，請確定它支援下列一組功能：  

- Switchport MTU 設定\(必要\)  
- MTU 設定為 > = 1674 位元組\(包括 L2 乙太網路標頭\)  
- L3 通訊協定\(必要\)  
- ECMP  
- BGP \(IETF RFC 4271\)\-基礎 ECMP

實作應該支援下列 IETF 標準必須陳述式。

- RFC 2545:「 針對 IPv6 網域間路由選擇 bgp-4 多重通訊協定擴充功能 」  
- RFC 4760:「 適用於 BGP 4 多重通訊協定擴充功能 」  
- RFC 4893:"四個八位元 AS 的 BGP 支援數字空間 」  
- RFC 4456:「 BGP 路由反映：替代完整網狀內部 BGP (IBGP) 」  
- RFC 4724:「 適用於 BGP 的依正常程序重新啟動機制 」  

下列標記的通訊協定所需。

- VLAN 為各種類型的流量隔離
- 802.1q 主幹

下列項目會提供連結的控制。

- 服務品質\(PFC 才需要使用 RoCE 如果\)
- 增強式流量選取\(802.1Qaz\)
- 優先順序型流量控制\(802.1 p/Q 和 802.1Qbb\)

下列項目提供可用性和備援能力。

- 切換 （必要） 的可用性
- 高可用性路由器，才能執行閘道函式。 您可以使用多底座 switch\ 路由器或 VRRP 等技術來執行這項操作。
        
下列項目會提供管理功能。

**監視**

- SNMP v1 或 SNMP v2 （如果使用網路控制卡的實體交換器監視需要）  
- SNMP Mib\(需要如果您使用網路控制卡的實體交換器監視\)  
- MIB-II (RFC 1213) LLDP，介面 MIB \(RFC 2863\)，IF-MIB、 IP MIB、 MIB-IP-順向、 Q-橋接器-MIB，橋接器 MIB、 LLDB MIB，實體 MIB、 IEEE8023-延隔時間-MIB  
  
下圖顯示四個節點設定範例。 為了清楚起見，第一個圖顯示只是網路控制站、 網路控制卡加上軟體負載平衡器，會顯示第二個和第三個圖表會顯示網路控制站、 軟體負載平衡器和閘道。  
  
這些圖表不會顯示儲存體網路和 Vnic。 如果您打算使用 SMB 存放裝置，這些是必要項目。
  
基礎結構和租用戶虛擬機器可轉散發 （假設為正確的邏輯網路有正確的網路連線） 的任何實體計算主機上。  
  

  
## <a name="switch-configuration-examples"></a>切換組態範例  
  
為了協助設定您的實體交換器或路由器，一組的各種不同的交換器模型和廠商的範例組態檔位於[Microsoft SDN Github 存放庫](https://github.com/microsoft/SDN/tree/master/SwitchConfigExamples)。 提供詳細的讀我檔案和已測試的命令列介面 (CLI) 命令，針對特定的參數。         
  
  
## <a name="compute"></a>計算  
所有的 HYPER-V 主機必須已安裝 Windows Server 2016、 啟用 HYPER-V，和至少一張實體介面卡以建立外部 HYPER-V 虛擬交換器連線至管理邏輯網路。 主機必須可透過管理 IP 位址指派給管理主機 vNIC。  
  
適用於 HYPER-V、 共用或本機任何儲存體類型都可用。   
  
> [!TIP]  
> 如果您使用相同的名稱，供您所有虛擬交換器，但不一定，很方便。 如果您打算部署使用指令碼，請參閱相關聯的註解`vSwitchName`config.psd1 檔案中的變數。  
  
**主機計算需求**  
下表顯示範例部署中所使用的四個實體主機的最小硬體和軟體需求。  
  
主機|硬體需求|軟體需求|  
--------|-------------------------|-------------------------  
|實體 Hyper-v 主機|4 核心 2.66 GHz CPU<br /><br />32 GB 的 RAM<br /><br />300 GB 的磁碟空間<br /><br />1 Gb/秒 （或更快） 實體網路介面卡|作業系統：Windows Server 2016<br /><br />安裝 HYPER-V 角色|  
  
  
**SDN 基礎結構虛擬機器角色需求**  
  
[角色]|vCPU 需求|記憶體需求|磁碟需求|  
--------|---------------------|-----------------------|---------------------  
|網路控制站 （三個節點）|4 個 Vcpu|4 GB 最小值 (建議採用 8 GB)|75 GB 的 OS 磁碟機  
|SLB/MUX （三個節點）|8 個 Vcpu|建議採用 8 GB|75 GB 的 OS 磁碟機  
|RAS 閘道<br /><br />（單一集區的三節點閘道、 兩個作用中、 一個被動）|8 個 Vcpu|建議採用 8 GB|75 GB 的 OS 磁碟機  
|RAS 閘道 BGP 路由器的 SLB/MUX 的對等互連<br /><br />（或者使用 ToR 交換器為 BGP 路由器）|2 個 Vcpu|2 GB|75 GB 的 OS 磁碟機|  
  
  
如果您使用 VMM 部署時，就有一個額外的基礎結構虛擬機器資源所 VMM 和其他非 SDN 基礎結構。 如需詳細資訊，請參閱[System Center Technical Preview 的最小硬體建議。](https://technet.microsoft.com/library/dn997303.aspx)  
  
## <a name="extending-your-infrastructure"></a>擴充您的基礎結構  
您的基礎結構的大小和資源需求會取決於您打算裝載租用戶工作負載虛擬機器。 CPU、 記憶體和磁碟需求的基礎結構虛擬機器 (例如： 網路控制站，SLB，閘道等)，如上表所述。 您可以新增多個這些的基礎結構虛擬機器，並視需要相應放大。 不過，在 HYPER-V 主機上執行任何租用戶虛擬機器會有自己的 CPU、 記憶體和磁碟需求，您必須考慮。   
  
當租用戶工作負載的虛擬機器開始耗用太多的資源，在實體 HYPER-V 主機上時，您可以新增額外的實體主機來擴充您的基礎結構。 做法是使用 Virtual Machine Manager 或使用 PowerShell 指令碼 （取決於您最初部署方式的基礎結構） 來建立新的伺服器資源，透過網路控制站。 如果您需要新增其他 IP 位址的 HNV 提供者網路，您可以建立新邏輯的子網路 （與對應的 IP 集區），主機可以使用。  
  
  
## <a name="see-also"></a>另請參閱  
[安裝和部署網路控制站準備需求](Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
[軟體定義網路&#40;SDN&#41;](../Software-Defined-Networking--SDN-.md)  
  


