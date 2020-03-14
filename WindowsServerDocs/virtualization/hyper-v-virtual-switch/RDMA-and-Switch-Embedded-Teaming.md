---
title: 遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)
description: 本主題提供有關使用 Windows Server 2016 中的 Hyper-v 來設定遠端直接記憶體存取（RDMA）介面的資訊，以及有關交換器內嵌小組（SET）的資訊。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b39cac842f115a1828c666eec52f17f80971510c
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322710"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>\(RDMA\) 的遠端直接記憶體存取和交換器內嵌小組 \(設定\)

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題提供有關使用 Windows Server 2016 中的 Hyper-v 來設定遠端直接記憶體存取 \(RDMA\) 介面的資訊，以及有關交換器內嵌小組 \(設定\)的資訊。  

> [!NOTE]
> 除了本主題之外，還有下列交換器內嵌小組內容可供使用。 
> - TechNet 元件庫下載： [Windows Server 2016 NIC 和交換器內嵌小組使用者指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>使用 Hyper-v 設定 RDMA 介面  

在 Windows Server 2012 R2 中，同時在提供 RDMA 服務的網路介面卡所在的同一部電腦上使用 RDMA 和 Hyper-v，無法系結至 Hyper-v 虛擬交換器。 這會增加需要安裝在 Hyper-v 主機中的實體網路介面卡數目。

>[!TIP]
>在 Windows server 2016 之前的 Windows Server 版本中，無法在系結至 NIC 小組或 Hyper-v 虛擬交換器的網路介面卡上設定 RDMA。 在 Windows Server 2016 中，您可以在已系結至 Hyper-v 虛擬交換器的網路介面卡上啟用 RDMA （不論是否已設定）。

在 Windows Server 2016 中，使用 RDMA 搭配或未設定時，您可以使用較少的網路介面卡。

下圖說明 Windows Server 2012 R2 與 Windows Server 2016 之間的軟體架構變更。

![架構變更](../media/RDMA-and-SET/rdma_over.jpg)

下列各節提供有關如何使用 Windows PowerShell 命令來啟用資料中心橋接（DCB）、使用 RDMA 虛擬 NIC 建立 Hyper-v 虛擬交換器 \(vNIC\)，以及使用 SET 和 RDMA Vnic 建立 Hyper-v 虛擬交換器的指示。

### <a name="enable-data-center-bridging-dcb"></a>啟用資料中心橋接 \(DCB\)

在使用聚合式乙太網路上的任何 RDMA \(RoCE\) 版本的 RDMA 之前，您必須啟用 DCB。  雖然網際網路範圍 RDMA 通訊協定不需要 \(iWARP\) 網路，但測試已判定所有以乙太網路為基礎的 RDMA 技術，都能更適合 DCB。 因此，您應該考慮使用 DCB，即使是 iWARP RDMA 部署也是如此。

下列 Windows PowerShell 範例命令示範如何啟用和設定 SMB 直接傳輸的 DCB。

開啟 DCB

    Install-WindowsFeature Data-Center-Bridging

設定 SMB 直接傳輸的原則：

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

開啟 SMB 的流量控制：

    Enable-NetQosFlowControl  -Priority 3

請確定流量控制針對其他流量關閉：

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

將原則套用至目標介面卡：

    Enable-NetAdapterQos  -Name "SLOT 2"

提供 SMB 直接傳輸最少的頻寬：

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

如果您已在系統中安裝內核偵錯工具，您必須將偵錯工具設定為允許透過執行下列命令來設定 QoS。

覆寫偵錯工具-根據預設，偵錯工具會封鎖 NetQos：
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>使用 RDMA vNIC 建立 Hyper-v 虛擬交換器

如果您的部署不需要 SET，您可以使用下列 Windows PowerShell 命令來建立具有 RDMA vNIC 的 Hyper-v 虛擬交換器。

> [!NOTE]
> 使用具有 RDMA 功能之實體 Nic 的集合小組，可提供更多 RDMA 資源供 Vnic 取用。

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

新增主機 Vnic，並讓它們能夠進行 RDMA：

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

驗證 RDMA 功能：

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>使用 SET 和 RDMA Vnic 建立 Hyper-v 虛擬交換器

若要使用 Hyper-v 主機虛擬網路介面卡上的 RDMA capabilies \(Vnic\) 在支援 RDMA 小組的 Hyper-v 虛擬交換器上，您可以使用這些範例 Windows PowerShell 命令。

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

新增主機 Vnic：

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

許多交換器不會在未標記的 VLAN 流量上傳遞流量類別資訊，因此請確定 RDMA 的主機介面卡是在 Vlan 上。 這個範例會將兩個 SMB_ * 主機虛擬配接器指派給 VLAN 42。
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

在主機 Vnic 上啟用 RDMA：

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

驗證 RDMA 功能;請確定這些功能不是零：

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>交換器內嵌小組（SET）  

本節概述 Windows Server 2016 中的交換器內嵌小組（SET），並包含下列各節。

- [設定總覽](#bkmk_over)

- [設定可用性](#bkmk_avail)

- [設定的支援和不支援的 Nic](#bkmk_nics)

- [設定與 Windows Server 網路技術的相容性](#bkmk_compat)

- [設定模式和設定](#bkmk_modes)

- [設定虛擬機器佇列（Vmq 數量）（& a）](#bkmk_vmq)

- [設定和 Hyper-v 網路虛擬化（HNV）](#bkmk_hnv)

- [設定和即時移轉](#bkmk_live)

- [MAC 位址在傳輸的封包上使用](#bkmk_mac)

- [管理集合小組](#bkmk_manage)

## <a name="bkmk_over"></a>設定總覽

SET 是替代的 NIC 小組解決方案，可用於包含 Hyper-v 的環境，以及在 Windows Server 2016 中 \(SDN\) 堆疊的軟體定義網路功能。 將一些 NIC 小組功能整合到 Hyper-v 虛擬交換器。

[設定] 可讓您將一或八個實體 Ethernet 網路介面卡組成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。

設定成員網路介面卡必須全部安裝在要放在小組中的相同實體 Hyper-v 主機。

> [!NOTE]
> 只有 Windows Server 2016 中的 Hyper-v 虛擬交換器才支援使用 SET。 您無法在 Windows Server 2012 R2 中部署 SET。

您可以將組合的 Nic 連接到相同的實體交換器或不同的實體交換器。 如果您將 Nic 連接至不同的交換器，這兩個交換器必須位於相同的子網上。

下圖描述設定架構。

![設定架構](../media/RDMA-and-SET/set_architecture.jpg)

由於 SET 已整合到 Hyper-v 虛擬交換器中，因此您無法在虛擬機器（VM）內部使用 SET。 不過，您可以在 Vm 中使用 NIC 小組。

如需詳細資訊，請參閱[虛擬機器（vm）中的 NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms)小組。

此外，SET 架構不會公開小組介面。 相反地，您必須設定 Hyper-v 虛擬交換器埠。

## <a name="bkmk_avail"></a>設定可用性

SET 適用于所有版本的 Windows Server 2016，包括 Hyper-v 和 SDN 堆疊。 此外，您可以使用 Windows PowerShell 命令和遠端桌面連線，從執行支援工具的用戶端作業系統的遠端電腦管理設定。

## <a name="bkmk_nics"></a>設定的支援 Nic

您可以使用已通過 Windows 硬體合格和標誌的任何 Ethernet NIC，\(WHQL\) 測試在 Windows Server 2016 中的一組小組。 設定需要所有屬於集合小組成員的網路介面卡都必須相同 \(也就是相同的製造商、相同的型號、相同的固件和驅動程式\)。 設定小組中的一到八個網路介面卡之間的支援。
  
## <a name="bkmk_compat"></a>設定與 Windows Server 網路技術的相容性

SET 與 Windows Server 2016 中的下列網路技術相容。

- 資料中心橋接 \(DCB\)
  
- Hyper-v 網路虛擬化-Windows Server 2016 支援 NV-GRE 和 VxLAN。  
- 接收端總和檢查碼卸載 \(IPv4、IPv6、TCP\)-如果有任何集合小組成員支援，則支援這些專案。

- \(RDMA\) 的遠端直接記憶體存取

- 單一根目錄 i/o 虛擬化 \(SR-IOV\)

- 傳輸端總和檢查碼卸載 \(IPv4、IPv6、TCP\)-如果所有集合小組成員都支援，則支援這些專案。

- 虛擬機器佇列 \(VMQ\)

- \(RSS\) 的虛擬接收端調整

SET 與 Windows Server 2016 中的下列網路技術不相容。

- 802.1 x 驗證。 802.1 x 可延伸驗證通訊協定 \(EAP\) 封包會在設定的案例中，由超\-V 虛擬交換器自動卸載。
 
- \(IPsecTO\)的 IPsec 工作卸載。 這是大多數網路介面卡不支援的舊版技術，而且存在的地方會預設為停用。

- 在主機或原生作業系統中使用 QoS \(pacer .exe\)。 這些 QoS 案例不是超\-V 案例，因此這些技術不會交集。 此外，QoS 也可以使用，但預設不會啟用-您必須刻意啟用 QoS。

- \(RSC\)的接收端聯合。 超\-V 虛擬交換器會自動停用 RSC。

- \(RSS\)接收端調整。 由於 Hyper-v 會使用 VMQ 和 VMMQ 的佇列，因此當您建立虛擬交換器時，一律會停用 RSS。

- TCP 煙囪卸載。 這項技術預設為停用。

- 虛擬機器 QoS \(VM-QoS\)。 VM QoS 已可使用，但預設為停用。 如果您在設定的環境中設定 VM QoS，QoS 設定會導致無法預期的結果。

## <a name="bkmk_modes"></a>設定模式和設定

與 NIC 小組不同的是，當您建立集合團隊時，無法設定小組名稱。 此外，NIC 小組也支援使用待命介面卡，但 SET 中不支援。 當您部署 SET 時，所有網路介面卡都處於作用中狀態，而且沒有處於待命模式。

NIC 小組與集合之間的另一個主要差異在於，NIC 小組可選擇三種不同的小組模式，而設定只支援**交換器獨立**模式。 使用 Switch 獨立模式時，集合小組成員所連接的參數就不會察覺設定小組是否存在，也不會決定如何散發網路流量以設定小組成員-相反地，集合小組會分配輸入網路跨設定小組成員的流量。

當您建立新的集合小組時，您必須設定下列小組屬性。

- 成員介面卡

- 負載平衡模式

### <a name="member-adapters"></a>成員介面卡

當您建立集合小組時，您必須指定最多八個與 Hyper-v 虛擬交換器系結的相同網路介面卡，以設定小組成員介面卡。

### <a name="load-balancing-mode"></a>負載平衡模式

[設定小組負載平衡] 分配模式的選項為 [ **Hyper-v 埠**] 和 [**動態**]。

**Hyper-v 埠**

Vm 會連線至 Hyper-v 虛擬交換器上的埠。 針對設定小組使用 Hyper-v 埠模式時，會使用 Hyper-v 虛擬交換器埠和相關聯的 MAC 位址來分割設定小組成員之間的網路流量。

> [!NOTE]
> 當您使用 SET 搭配封包直接傳輸時，小組模式**交換器會獨立**，而且需要負載平衡模式**hyper-v 通訊埠**。

由於連續的交換器一律會在指定的埠上看到特定的 MAC 位址，因此交換器會將輸入負載（從交換器到主機的流量）散發到 MAC 位址所在的埠。 這在使用虛擬機器佇列（Vmq 數量）時特別有用，因為佇列可以放在預期抵達流量的特定 NIC 上。

不過，如果主機只有幾個 Vm，則此模式可能不夠細微，而無法達到良好平衡的散發。 此模式也一律會將單一 VM （也就是來自單一交換器埠的流量）限制為單一介面上可用的頻寬。

**效果**

此負載平衡模式提供下列優點。

- 輸出負載是根據 TCP 通訊埠和 IP 位址的雜湊來散發。  動態模式也會即時重新平衡負載，讓指定的輸出流程可以在集合小組成員之間來回移動。

- 輸入負載的散發方式與 Hyper-v 埠模式相同。

在此模式中的輸出負載，會根據 flowlets 的概念進行動態平衡。 正如人類語音在單字和句子的結尾處自然中斷，TCP 流量（TCP 通訊串流）也會發生自然的中斷。 兩個這類中斷之間的 TCP 流程部分稱為「flowlet」。

當動態模式演算法偵測到已遇到的 flowlet 界限時（例如，當 TCP 流程中發生了足夠長度的中斷）時，演算法會自動將流程重新平衡至另一個小組成員（如果有的話）。  在某些罕見的情況下，此演算法可能也會定期重新平衡不包含任何 flowlets 的流程。 因此，TCP 流程與小組成員之間的親和性可能會隨時變更，因為動態平衡演算法會運作，以平衡小組成員的工作負載。

## <a name="bkmk_vmq"></a>設定虛擬機器佇列（Vmq 數量）（& a）

VMQ 並妥善設定工作，而且您應該在每次使用 Hyper-v 並設定時啟用 VMQ。

> [!NOTE]
> [設定] 一律會顯示所有集合小組成員可用的佇列總數。 在 NIC 小組中，這稱為「佇列的總和」模式。

大部分的網路介面卡都有佇列，可用於 \(RSS\) 或 VMQ 的接收端調整，但不能同時用於兩者。
  
某些 VMQ 設定似乎是 RSS 佇列的設定，但實際上是根據目前使用中的功能，而在一般佇列上的設定。 每個 NIC 都在其 [advanced] 屬性中，`*RssBaseProcNumber` 和 `*MaxRssProcessors`的值。

以下是一些可提供較佳系統效能的 VMQ 設定。

- 在理想情況下，每個 NIC 的 `*RssBaseProcNumber` 都應該設為大於或等於二（2）的偶數數位。 這是因為第一個實體處理器（核心0） \(邏輯處理器0和 1\)，通常會執行大部分的系統處理，因此應該從這個實體處理器操縱網路處理。 

>[!NOTE]
>某些機器架構的每個實體處理器不會有兩個邏輯處理器，因此，針對這類機器，基底處理器應大於或等於1。 如果不確定，請假設您的主機使用每個實體處理器架構2個邏輯處理器。

- 小組成員的處理器應該是實際、非重迭的範圍。 例如，在4核心主機 \(8 個邏輯處理器\) 與2個 10Gbps Nic 的小組一起使用時，您可以將第一個設定為使用基底處理器2，並使用4個核心;第二個設定為使用基本處理器6，並使用2個核心。

## <a name="bkmk_hnv"></a>\(HNV\) 設定和 Hyper-v 網路虛擬化

SET 與 Windows Server 2016 中的 Hyper-v 網路虛擬化完全相容。 HNV 管理系統會提供資訊給 SET 驅動程式，允許設定以針對 HNV 流量優化的方式來分散網路流量負載。
  
## <a name="bkmk_live"></a>設定和即時移轉

Windows Server 2016 支援即時移轉。

## <a name="bkmk_mac"></a>MAC 位址在傳輸的封包上使用

當您使用動態負載分佈來設定集合小組時，來自單一來源 \(（例如單一 VM\)）的封包會同時分散到多個小組成員。 

若要防止交換器混淆並防止 MAC flapping 警示，請在相似化為小組成員以外的小組成員上，設定以不同的 MAC 位址取代來源 MAC 位址。 因此，每個小組成員都使用不同的 MAC 位址，而且除非發生失敗，否則會阻止 MAC 位址衝突。

在主要 NIC 上偵測到失敗時，集合小組軟體會開始使用已選擇做為暫存相似化為小組成員之團隊成員上的 VM MAC 位址，\(也就是，現在會顯示為 VM 介面\)的交換器。

這種變更僅適用于在 VM 的相似化為小組成員上傳送的流量，其具有 VM 自己的 MAC 位址作為其來源 MAC 位址。 其他流量會繼續與失敗前所使用的任何來源 MAC 位址一起傳送。

下列清單會根據小組的設定方式來描述集合分組 MAC 位址取代行為：

- 使用 Hyper-v 通訊埠發佈的交換器獨立模式

    - 每個 vmSwitch 埠會相似化為至小組成員
  
    - 每個封包都會在相似化為埠的小組成員上傳送  
  
    - 未完成來源 MAC 取代  
  
- 使用動態散發的交換器獨立模式
  
    - 每個 vmSwitch 埠會相似化為至小組成員  
  
    - 所有 ARP/NS 封包都會傳送至相似化為埠的小組成員  
  
    - 在相似化為小組成員的小組成員上傳送的封包未完成來源 MAC 位址取代  
  
    - 在相似化為小組成員以外的小組成員上傳送的封包將會完成來源 MAC 位址取代  
  
## <a name="bkmk_manage"></a>管理集合小組

建議您使用 System Center Virtual Machine Manager \(VMM\) 來管理集合小組，不過您也可以使用 Windows PowerShell 來管理集合。 下列各節提供可用來管理 SET 的 Windows PowerShell 命令。

如需有關如何使用 VMM 建立集合小組的詳細資訊，請參閱 System Center VMM 程式庫主題中的「設定邏輯交換器」一節，[建立邏輯交換器](https://docs.microsoft.com/system-center/vmm/network-switch)。
  
### <a name="create-a-set-team"></a>建立集合小組

您必須在建立 Hyper-v 虛擬交換器時，使用**新的 VMSwitch** Windows PowerShell 命令來建立集合小組。

當您建立 Hyper-v 虛擬交換器時，您必須在命令語法中包含新的**EnableEmbeddedTeaming**參數。 在下列範例中，會建立名為**TeamedvSwitch**且具有內嵌小組和兩個初始團隊成員的 hyper-v 交換器。
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
當**NetAdapterName**的引數是 nic 的陣列，而不是單一 nic 時，Windows PowerShell 會採用**EnableEmbeddedTeaming**參數。 因此，您可以透過下列方式修改先前的命令。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

如果您想要建立具有單一小組成員的可設定參數，讓您可以在稍後新增小組成員，則必須使用 EnableEmbeddedTeaming 參數。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>加入或移除集合小組成員

**VMSwitchTeam**命令包含**NetAdapterName**選項。 若要變更集合小組中的小組成員，請在 [ **NetAdapterName** ] 選項後面輸入想要的小組成員清單。 如果**TeamedvSwitch**最初是使用 nic 1 和 nic 2 建立的，則下列範例命令會刪除設定小組成員 "nic 2"，並加入新的集合小組成員 "nic 3"。
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>移除集合小組

您只需移除包含集合小組的 Hyper-v 虛擬交換器，即可移除集合小組。  如需如何移除 Hyper-v 虛擬交換器的詳細資訊，請使用[移除-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch)主題。 下列範例會移除名為**SETvSwitch**的虛擬交換器。

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>變更集合小組的負載分佈演算法

**VMSwitchTeam** Cmdlet 具有**LoadBalancingAlgorithm**選項。 此選項會接受兩個可能值的其中一個： **HyperVPort**或**Dynamic**。 若要設定或變更交換器內嵌小組的負載分佈演算法，請使用此選項。 

在下列範例中，名為**TeamedvSwitch**的 VMSwitchTeam 會使用**動態**負載平衡演算法。  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>關聯實體小組成員的虛擬介面

設定可讓您在虛擬介面之間建立親和性 \(例如，Hyper-v 虛擬交換器埠\) 和小組中的其中一個實體 Nic。 

例如，如果您建立兩個 SMB\-Direct 的主機 Vnic，如使用[SET 和 RDMA Vnic 建立 Hyper-v 虛擬交換器](#bkmk_set-rdma)一節中所述，您可以確定這兩個 vnic 使用不同的小組成員。 

將新增至該區段中的腳本，您可以使用下列 Windows PowerShell 命令。

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

本主題將在[Windows Server 2016 NIC 和交換器內嵌小組使用者指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)的4.2.5 一節中深入探討。
