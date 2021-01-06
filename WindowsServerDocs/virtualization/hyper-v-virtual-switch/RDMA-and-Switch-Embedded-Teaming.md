---
title: 遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)
description: 本主題提供的資訊說明如何在 Windows Server 2016 中設定 Hyper-v 的遠端直接記憶體存取 (RDMA) 介面，以及切換內嵌團隊 (設定) 的相關資訊。
manager: brianlic
ms.topic: how-to
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: lizross
author: eross-msft
ms.date: 12/08/2020
ms.openlocfilehash: f671496cab42cc24c35539d5adbfaff7fb90bea7
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946904"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>遠端直接記憶體存取 \( RDMA \) 和交換器內嵌小組 \( 集合\)

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題提供有關在 Windows Server 2016 中設定 Hyper-v 的遠端直接記憶體存取 \( RDMA 介面的資訊 \) ，以及切換內嵌小組集的相關資訊 \( \) 。

> [!NOTE]
> 除了本主題之外，還提供下列交換器內嵌小組內容。
> - TechNet 元件庫下載： [Windows Server 2016 NIC 和交換器 Embedded 小組使用者指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="configuring-rdma-interfaces-with-hyper-v"></a><a name="bkmk_rdma"></a>使用 Hyper-v 設定 RDMA 介面

在 Windows Server 2012 R2 中，在同一部電腦上同時使用 RDMA 和 Hyper-v 作為提供 RDMA 服務的網路介面卡，不能系結至 Hyper-v 虛擬交換器。 這會增加在 Hyper-v 主機中安裝所需的實體網路介面卡數目。

>[!TIP]
>在 Windows server 2016 之前的 Windows Server 版本中，無法在系結至 NIC 小組或 Hyper-v 虛擬交換器的網路介面卡上設定 RDMA。 在 Windows Server 2016 中，您可以在系結至 Hyper-v 虛擬交換器（不論是否已設定）的網路介面卡上啟用 RDMA。

在 Windows Server 2016 中，使用 RDMA 搭配或未設定時，您可以使用較少的網路介面卡。

下圖說明 Windows Server 2012 R2 和 Windows Server 2016 之間的軟體架構變更。

![架構變更](../media/RDMA-and-SET/rdma_over.jpg)

下列各節提供如何使用 Windows PowerShell 命令以啟用資料中心橋接 (DCB) 、使用 RDMA 虛擬 NIC vNIC 建立 Hyper-v 虛擬交換器 \( \) ，以及使用 SET 和 RDMA Vnic 建立 Hyper-v 虛擬交換器的指示。

### <a name="enable-data-center-bridging-dcb"></a>啟用資料中心橋接 \( DCB\)

在透過交集 Ethernet \( RoCE 版本的 rdma 使用任何 rdma 之前 \) ，您必須先啟用 DCB。  雖然網際網路廣域網路區域 RDMA 通訊協定 iWARP 網路並不需要 \( \) ，但測試已判定所有以乙太網路為基礎的 rdma 技術在 DCB 方面都能更有效地運作。 基於這個原因，您應該考慮使用 DCB，即使是 iWARP RDMA 部署也是如此。

下列 Windows PowerShell 命令範例示範如何啟用和設定 SMB Direct 的 DCB。

開啟 DCB

```powershell
Install-WindowsFeature Data-Center-Bridging
```

設定 SMB 直接存取的原則：

```powershell
New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
```

開啟 SMB 的流程式控制制：

```powershell
Enable-NetQosFlowControl  -Priority 3
```

請確定流量控制針對其他流量關閉：

```powershell
Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7
```

將原則套用至目標介面卡：

```powershell
Enable-NetAdapterQos  -Name "SLOT 2"
```

至少為 SMB 提供最少頻寬的30%：

```powershell
New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS
```

如果您已在系統中安裝內核偵錯工具，則必須將偵錯工具設定為允許透過執行下列命令來設定 QoS。

覆寫偵錯工具-根據預設，偵錯工具區塊 NetQos：

```powershell
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
```

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>使用 RDMA vNIC 建立 Hyper-v 虛擬交換器

如果您的部署不需要設定，您可以使用下列 Windows PowerShell 命令建立具有 RDMA vNIC 的 Hyper-v 虛擬交換器。

> [!NOTE]
> 使用具有支援 RDMA 之實體 Nic 的集合小組，可提供更多 RDMA 資源讓 Vnic 取用。

```powershell
New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"
```

新增主機 Vnic，並使其可支援 RDMA：

```powershell
Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"
```
確認 RDMA 功能：

```powershell
Get-NetAdapterRdma
```

###  <a name="create-a-hyper-v-virtual-switch-with-set-and-rdma-vnics"></a><a name="bkmk_set-rdma"></a>使用 SET 和 RDMA Vnic 建立 Hyper-v 虛擬交換器

若要在支援 RDMA 小組的 Hyper-v 虛擬交換器上 vnic 的 Hyper-v 主機虛擬網路介面卡上使用 RDMA capabilies \( \) ，您可以使用這些範例 Windows PowerShell 命令。

```powershell
New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true
```

新增主機 Vnic：

```powershell
Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS
```

許多參數都不會將流量類別資訊傳遞到未標記的 VLAN 流量，因此請確定 RDMA 的主機介面卡是在 Vlan 上。 此範例會將兩個 SMB_ * 主機虛擬配接器指派給 VLAN 42。

```powershell
Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
```

在主機 Vnic 上啟用 RDMA：

```powershell
Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"
```

確認 RDMA 功能;確定這些功能不是零：

```powershell
Get-NetAdapterRdma | fl *
```

## <a name="switch-embedded-teaming-set"></a>切換內嵌團隊 (設定) 

本節概述 Windows Server 2016 中的交換器內嵌小組 (設定) ，其中包含下列各節。

- [設定總覽](#bkmk_over)

- [設定可用性](#bkmk_avail)

- [支援和不支援的設定 Nic](#bkmk_nics)

- [設定與 Windows Server 網路技術的相容性](#bkmk_compat)

- [設定模式和設定](#bkmk_modes)

- [ (VMQs) 設定和虛擬機器佇列 ](#bkmk_vmq)

- [ (HNV) 設定和 Hyper-v 網路虛擬化 ](#bkmk_hnv)

- [設定和即時移轉](#bkmk_live)

- [MAC 位址在傳輸的封包上使用](#bkmk_mac)

- [管理集合小組](#bkmk_manage)

## <a name="set-overview"></a><a name="bkmk_over"></a>設定總覽

SET 是替代的 NIC 小組解決方案，可在 \( \) Windows Server 2016 中包含 Hyper-v 和軟體定義網路 SDN 堆疊的環境中使用。 SET 可將某些 NIC 小組功能整合到 Hyper-v 虛擬交換器。

設定可讓您在一或多個實體 Ethernet 網路介面卡之間進行分組，以組成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。

設定成員網路介面卡必須全部安裝在相同的實體 Hyper-v 主機中，才能放置在小組中。

> [!NOTE]
> 只有 Windows Server 2016 中的 Hyper-v 虛擬交換器支援使用 SET。 您無法在 Windows Server 2012 R2 中部署 SET。

您可以將組合的 Nic 連線到相同的實體交換器或不同的實體交換器。 如果您將 Nic 連線至不同的交換器，這兩個交換器都必須位於相同的子網上。

下圖描述設定的架構。

![設定架構](../media/RDMA-and-SET/set_architecture.jpg)

因為 SET 已整合到 Hyper-v 虛擬交換器中，所以您無法在虛擬機器中使用 SET (VM) 。 不過，您可以在 Vm 中使用 NIC 小組。

如需詳細資訊，請參閱 [虛擬機器中的 NIC 小組 (vm) ](../../networking/technologies/nic-teaming/nic-teaming.md)。

此外，SET 架構也不會公開小組介面。 相反地，您必須設定 Hyper-v 虛擬交換器埠。

## <a name="set-availability"></a><a name="bkmk_avail"></a>設定可用性

SET 適用于所有包含 Hyper-v 和 SDN 堆疊的 Windows Server 2016 版本。 此外，您可以使用 Windows PowerShell 命令和遠端桌面連線，從執行支援工具之用戶端作業系統的遠端電腦管理設定。

## <a name="supported-nics-for-set"></a><a name="bkmk_nics"></a>設定的支援 Nic

您可以使用已 \( \) 在 windows Server 2016 的集合團隊中通過 Windows 硬體認證和商標 WHQL 測試的任何 Ethernet NIC。 設定要求所有屬於設定小組成員的網路介面卡都必須相同， \( 亦即相同的製造商、相同的型號、相同的固件和驅動程式 \) 。 在小組中的一到八張網路介面卡設定支援。

## <a name="set-compatibility-with-windows-server-networking-technologies"></a><a name="bkmk_compat"></a>設定與 Windows Server 網路技術的相容性

SET 與 Windows Server 2016 中的下列網路技術相容。

- 資料中心橋接 \( DCB\)

- Hyper-v 網路虛擬化-NV-GRE 和 VxLAN 都支援 Windows Server 2016。
- 接收端總和檢查碼 \( 會卸載 IPv4、IPv6、TCP \) -如果有任何集合的小組成員支援，就會支援這些專案。

- 遠端直接記憶體存取 \( RDMA\)

- 單一根目錄 i/o 虛擬化 \( sr-iov\)

- 傳輸端總和檢查碼 \( 會卸載 IPv4、IPv6、TCP \) -如果所有的集合成員都支援這些設定，則支援這些專案。

- 虛擬機器佇列 \( VMQ\)

- 虛擬接收端調整 \( RSS\)

設定與 Windows Server 2016 中的下列網路技術不相容。

- 802.1 x 驗證。 802.1 x 可延伸驗證通訊協定 \( EAP 封 \) 包會 \- 在設定的情況下，由 hyper-v 虛擬交換器自動卸載。

- IPsec 工作卸載 \( IPsecTO \) 。 這是大部分網路介面卡不支援的舊版技術，而且它存在的位置預設為停用。

- \( \) 在主機或原生作業系統中使用 QoSpacer.exe。 這些 QoS 案例不是 Hyper-v \- 案例，因此技術不會交集。 此外，QoS 可供使用，但預設不會啟用-您必須刻意啟用 QoS。

- 接收端聯合 \( RSC \) 。 Hyper-v 虛擬交換器會自動停用 RSC \- 。

- 接收端調整 \( RSS \) 。 因為 Hyper-v 使用 VMQ 和 VMMQ 的佇列，所以當您建立虛擬交換器時，一律會停用 RSS。

- TCP Chimney 卸載。 此技術預設為停用。

- 虛擬機器 QoS \( VM-qos \) 。 VM QoS 可供使用，但預設為停用。 如果您在設定的環境中設定 VM QoS，QoS 設定將會導致無法預期的結果。

## <a name="set-modes-and-settings"></a><a name="bkmk_modes"></a>設定模式和設定

與 NIC 小組不同的是，當您建立集合小組時，無法設定小組名稱。 此外，NIC 小組也支援使用待命介面卡，但設定中不支援。 當您部署 SET 時，所有網路介面卡都處於作用中狀態，而且沒有處於待命模式。

NIC 小組與設定之間的另一個主要差異是，NIC 小組可以選擇三種不同的小組模式，而設定只支援 **切換獨立** 模式。 使用切換獨立模式時，設定小組成員所連接的參數不會察覺設定小組是否存在，也不會決定如何將網路流量散發到設定小組成員，而是由集合團隊將輸入的網路流量分散到集合的小組成員。

當您建立新的集合小組時，您必須設定下列小組屬性。

- 成員介面卡

- 負載平衡模式

### <a name="member-adapters"></a>成員介面卡

當您建立集合小組時，您必須指定最多8個與 Hyper-v 虛擬交換器系結的相同網路介面卡，例如設定的小組成員介面卡。

### <a name="load-balancing-mode"></a>負載平衡模式

設定小組負載平衡分配模式的選項是 **Hyper-v 埠** 和 **動態**。

**Hyper-v 埠**

Vm 會連線至 Hyper-v 虛擬交換器上的埠。 針對設定的小組使用 Hyper-v 埠模式時，會使用 Hyper-v 虛擬交換器埠和相關聯的 MAC 位址來分割集合小組成員之間的網路流量。

> [!NOTE]
> 當您搭配使用 SET 與封包直接時，小組模式  **交換器是獨立** 的，且需要負載平衡模式 **hyper-v 埠** 。

由於連續的交換器一律會在指定的埠上看見特定的 MAC 位址，因此交換器會將輸入負載 (從切換至主機) 到主機的流量，以找出 MAC 位址所在的埠。 這在使用虛擬機器佇列 (VMQs) 時特別有用，因為您可以將佇列放在預期流量抵達的特定 NIC 上。

不過，如果主機只有少數 Vm，此模式可能不夠細微，無法達成妥善平衡的散發。 此模式也一律會限制單一 VM (例如，從單一交換器埠的流量) 到單一介面上可用的頻寬。

**動態**

此負載平衡模式提供下列優點。

- 輸出負載是根據 TCP 埠和 IP 位址的雜湊來散發。  動態模式也會即時重新平衡負載，讓指定的輸出流程可以在設定的小組成員之間來回移動。

- 輸入負載的散發方式與 Hyper-v 埠模式相同。

這種模式的輸出負載會根據 flowlets 的概念進行動態平衡。 就像人類語音在單字和句子的結尾處有自然的中斷，TCP 流量 (的 TCP 通訊串流) 也會出現自然的中斷。 在兩個這類中斷之間的 TCP 流量的部分稱為 flowlet。

當動態模式演算法偵測到已發生 flowlet 界限時（例如，當 TCP 流程中發生足夠的長度中斷時），演算法會自動將流程重新平衡到另一個小組成員（如果有的話）。  在某些罕見的情況下，演算法也可能會定期重新平衡不包含任何 flowlets 的流程。 因此，TCP 流程和小組成員之間的親和性可隨時變更，因為動態平衡演算法可進行工作負載的負載平衡。

## <a name="set-and-virtual-machine-queues-vmqs"></a><a name="bkmk_vmq"></a> (VMQs) 設定和虛擬機器佇列

VMQ 並將工作妥善設定在一起，而且您應該在每次使用 Hyper-v 並設定時啟用 VMQ。

> [!NOTE]
> SET 一律會顯示所有已設定之小組成員的可用佇列總數。 在 NIC 小組中，這稱為「佇列的總計」模式。

大部分的網路介面卡都有可用於接收端調整 \( RSS \) 或 VMQ 的佇列，但不能同時使用兩者。

某些 VMQ 設定似乎是 RSS 佇列的設定，但實際上是根據目前正在使用的功能，在 RSS 和 VMQ 使用的一般佇列上的設定。 每個 NIC 在其 advanced 屬性中都有和的值 `*RssBaseProcNumber` `*MaxRssProcessors` 。

以下是一些 VMQ 設定，可提供更好的系統效能。

- 在理想的情況下，每個 NIC 都應該將 `*RssBaseProcNumber` 設定為大於或等於兩個 (2) 的偶數。 這是因為第一個實體處理器（核心 0 \( 邏輯處理器0和 1 \) ）通常會執行大部分的系統處理，因此應該將網路處理操縱從此實體處理器之外。

>[!NOTE]
>某些機器架構的每個實體處理器都不會有兩個邏輯處理器，因此針對這類機器，基底處理器應大於或等於1。 如果有疑問，請假設您的主機在每個實體處理器架構中使用2個邏輯處理器。

- 小組成員的處理器應該是其實際、非重迭的範圍。 例如，在 \( 具有2個 10Gbps nic 小組的4核心主機8邏輯處理器中 \) ，您可以將第一個設定為使用2的基底處理器並使用4個核心; 第二個設定為使用基本處理器6，並使用2個核心。

## <a name="set-and-hyper-v-network-virtualization-hnv"></a><a name="bkmk_hnv"></a>設定 Hyper-v 網路虛擬化 HNV （& V） \(\)

SET 與 Windows Server 2016 中的 Hyper-v 網路虛擬化完全相容。 HNV 管理系統提供設定驅動程式的資訊，可讓您以針對 HNV 流量優化的方式來分散網路流量負載。

## <a name="set-and-live-migration"></a><a name="bkmk_live"></a>設定和即時移轉

Windows Server 2016 中支援即時移轉。

## <a name="mac-address-use-on-transmitted-packets"></a><a name="bkmk_mac"></a>MAC 位址在傳輸的封包上使用

當您設定具有動態負載分佈的集合小組時，來自單一 VM 等單一來源的封包 \( \) 會同時分散到多個小組成員。

若要防止交換器混淆並防止 MAC flapping 警示，請在相似化為小組成員以外的小組成員上，設定以不同 MAC 位址取代來源 MAC 位址。 因此，每個小組成員都會使用不同的 MAC 位址，而且除非發生失敗，否則會防止 MAC 位址衝突。

在主要 NIC 上偵測到失敗時，設定小組軟體會開始使用已選擇作為暫時性相似化為小組成員之小組成員上的 VM MAC 位址 \( ，亦即，現在會以 vm 介面形式出現在交換器上的成員。 \)

這種變更只適用于即將在 VM 的相似化為小組成員上傳送的流量，並以 VM 的 MAC 位址作為來源 MAC 位址。 其他流量會繼續以在失敗前所使用的任何來源 MAC 位址來傳送。

以下是根據小組的設定方式來描述設定小組 MAC 位址取代行為的清單：

- 使用 Hyper-v 埠散發的交換器獨立模式

    - 每個 vmSwitch 埠都會相似化為至小組成員

    - 每個封包都會在通訊埠相似化為的小組成員上傳送

    - 未完成來源 MAC 更換

- 使用動態散發的交換器獨立模式

    - 每個 vmSwitch 埠都會相似化為至小組成員

    - 所有 ARP/NS 封包都會傳送到埠所相似化為的小組成員

    - 在相似化為小組成員的小組成員上傳送的封包未完成來源 MAC 位址取代

    - 在相似化為小組成員以外的小組成員上傳送的封包將會完成來源 MAC 位址取代

## <a name="managing-a-set-team"></a><a name="bkmk_manage"></a>管理集合小組

建議您使用 System Center Virtual Machine Manager \( VMM \) 來管理設定的小組，但您也可以使用 Windows PowerShell 來管理設定。 下列各節提供可用來管理集合的 Windows PowerShell 命令。

如需有關如何使用 VMM 建立集合團隊的詳細資訊，請參閱 System Center VMM 程式庫主題中的「設定邏輯交換器」一節， [建立邏輯交換器](/system-center/vmm/network-switch)。

### <a name="create-a-set-team"></a>建立集合小組

您必須在建立 Hyper-v 虛擬交換器時，使用 **新的-VMSwitch** Windows PowerShell 命令建立集合團隊。

當您建立 Hyper-v 虛擬交換器時，您必須在命令語法中包含新的 **EnableEmbeddedTeaming** 參數。 在下列範例中，會建立名為  **TeamedvSwitch** 的 hyper-v 交換器和內嵌小組，以及兩個初始團隊成員。

```
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true
```

當 **NetAdapterName** 的引數是 nic 的陣列，而不是單一 nic 時，Windows PowerShell 會假設 **EnableEmbeddedTeaming** 參數。 因此，您可以透過下列方式修改先前的命令。

```
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"
```

如果您想要使用單一小組成員建立具有集合功能的參數，讓您可以在稍後加入小組成員，您必須使用 EnableEmbeddedTeaming 參數。

```
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true
```

### <a name="adding-or-removing-a-set-team-member"></a>加入或移除集合小組成員

**VMSwitchTeam** 命令包含 **NetAdapterName** 選項。 若要變更集合團隊中的小組成員，請在 [ **NetAdapterName** ] 選項之後，輸入所需的小組成員清單。 如果 **TeamedvSwitch** 原本是使用 nic 1 和 nic 2 建立的，則下列範例命令會刪除設定的小組成員 "nic 2"，並加入新的集合小組成員 "nic 3"。

```
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"
```

### <a name="removing-a-set-team"></a>移除集合小組

您可以移除包含集合小組的 Hyper-v 虛擬交換器，以移除集合團隊。  如需有關如何移除 Hyper-v 虛擬交換器的詳細資訊，請使用 [移除-VMSwitch](/powershell/module/hyper-v/remove-vmswitch) 主題。 下列範例會移除名為 **SETvSwitch** 的虛擬交換器。

```
Remove-VMSwitch "SETvSwitch"
```

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>變更集合小組的負載分佈演算法

**VMSwitchTeam** Cmdlet 有 **LoadBalancingAlgorithm** 選項。 此選項會採用兩個可能值的其中一個： **HyperVPort** 或 **Dynamic**。 若要設定或變更切換內嵌團隊的負載分配演算法，請使用此選項。

在下列範例中，名為 **TeamedvSwitch** 的 VMSwitchTeam 會使用 **動態** 負載平衡演算法。
```
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic
```
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>關聯實體團隊成員的虛擬介面

設定可讓您在虛擬介面（也就是 \( Hyper-v 虛擬交換器埠 \) 和團隊中的其中一個實體 nic）之間建立親和性。

例如，如果您建立了兩個適用于 SMB Direct 的主機 Vnic \- ，如使用 [SET 和 RDMA Vnic 建立 Hyper-v 虛擬交換器](#bkmk_set-rdma)一節中所述，您可以確定這兩個 vnic 使用不同的小組成員。

新增至該區段中的腳本，您可以使用下列 Windows PowerShell 命令。

```powershell
Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”
```

本主題將在 [Windows Server 2016 NIC 和交換器 Embedded 小組使用者指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)的章節4.2.5 中深入探討。
