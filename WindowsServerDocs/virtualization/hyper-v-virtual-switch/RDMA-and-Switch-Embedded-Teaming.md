---
title: 遠端直接記憶體存取 (RDMA) 和交換器內嵌小組 (SET)
description: 本主題提供使用 Windows Server 2016 中的 HYPER-V 設定遠端直接記憶體存取 (RDMA) 介面，除了資訊有關 Switch Embedded Teaming (SET) 的資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c945144194ac86623bfa8ce60a306f0202df0874
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881659"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>遠端直接記憶體存取\(RDMA\)和交換器內嵌小組\(設定\)

>適用於：Windows Server （半年通道），Windows Server 2016

本主題提供有關設定遠端直接記憶體存取\(RDMA\)另外介面與 Windows Server 2016 中的 HYPER-V 交換器內嵌小組的資訊\(設定\)。  

> [!NOTE]
> 本主題中，除了下列的交換器內嵌小組內容使用。 
> - TechNet 組件庫下載：[Windows Server 2016 NIC 和交換器內嵌小組的使用者指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>踏上雲端的設定 RDMA 介面  

在 Windows Server 2012 R2 中，使用 RDMA 和 HYPER-V 提供 RDMA 服務可以未繫結至 HYPER-V 虛擬交換器的網路介面卡的同一部電腦上。 這會增加，才能安裝在 HYPER-V 主機的實體網路介面卡的數目。

>[!TIP]
>在 Windows Server 2016 之前的 Windows Server 版本，不可能會繫結至 NIC 小組，或 HYPER-V 虛擬交換器的網路介面卡上設定 RDMA。 在 Windows Server 2016 中，您可以繫結至 HYPER-V 虛擬交換器包含或不含設定的網路介面卡上啟用 RDMA。

在 Windows Server 2016 中，您可以使用較少的網路介面卡時使用 RDMA，不論有無組。

下圖說明 Windows Server 2012 R2 和 Windows Server 2016 之間的軟體架構變更。

![架構變更](../media/RDMA-and-SET/rdma_over.jpg)

下列各節提供有關如何使用 Windows PowerShell 命令來啟用資料中心橋接 (DCB)、 使用 RDMA 虛擬 NIC 建立 HYPER-V 虛擬交換器的指示\(vNIC\)，並建立 HYPER-V 虛擬交換器設定和 RDMA Vnic。

### <a name="enable-data-center-bridging-dcb"></a>啟用資料中心橋接\(DCB\)

之前使用透過聚合式乙太網路的任何 RDMA \(RoCE\)版本的 RDMA，您必須啟用 DCB。  雖然不需要網際網路寬區域 RDMA 通訊協定\(iWARP\)測試所有的乙太網路為基礎的 RDMA 技術能更加配合 DCB 判定網路。 基於這個原因，您應該考慮使用 DCB 甚至 iWARP RDMA 部署。

下列 Windows PowerShell 的範例命令示範如何啟用及設定 SMB 直接傳輸 DCB。

開啟 DCB

    Install-WindowsFeature Data-Center-Bridging

SMB 直接傳輸，設定原則︰

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

開啟 SMB 的流量控制：

    Enable-NetQosFlowControl  -Priority 3

請確定其他流量流程控制已關閉：

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

原則套用至目標介面卡：

    Enable-NetAdapterQos  -Name "SLOT 2"

提供 SMB Direct 的 30%的最小頻寬：

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

如果您有安裝在系統的核心偵錯時，您必須設定偵錯工具，以允許執行下列命令來設定 QoS。

偵錯工具會覆寫-根據預設偵錯工具會封鎖 NetQos:
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>使用 RDMA vNIC 建立 HYPER-V 虛擬交換器

如果設定不需要為您的部署，您可以使用下列 Windows PowerShell 命令以使用 RDMA vNIC 建立 HYPER-V 虛擬交換器。

> [!NOTE]
> 使用具備 RDMA 功能的實體 Nic 設定小組提供 vnic 取用更多的 RDMA 資源。

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

新增主機 Vnic，並使其 RDMA 支援：

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

確認 RDMA 功能：

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>使用 SET 和 RDMA Vnic 建立 HYPER-V 虛擬交換器

若要利用 RDMA HYPER-V 上的服務裝載虛擬網路介面卡\(Vnic\)上支援 RDMA 的組合為 HYPER-V 虛擬交換器，您可以使用這些範例 Windows PowerShell 命令。

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

將主機 Vnic 新增：

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

許多參數將不會傳遞對未標記的 VLAN 網路流量的流量類別資訊，因此請確定主機介面卡的 RDMA 是 vlan。 此範例中指派的兩個 SMB_ * 主機虛擬介面卡的 VLAN 42。
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

在主機 Vnic 上啟用 RDMA:

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

確認 RDMA 功能;請確定功能為非零：

    Get-NetAdapterRdma | fl *


## <a name="bkmk_sswitchembedded"></a>交換器內嵌小組 (SET)  

本節概述的 Switch Embedded Teaming (SET) 在 Windows Server 2016 中，並包含下列各節。

- [設定概觀](#bkmk_over)

- [設定可用性](#bkmk_avail)

- [支援和不支援 Nic 設定](#bkmk_nics)

- [設定與 Windows 伺服器網路技術的相容性](#bkmk_compat)

- [設定模式和設定](#bkmk_modes)

- [設定和虛擬機器佇列 (Vmq)](#bkmk_vmq)

- [設定與 HYPER-V 網路虛擬化 (HNV)](#bkmk_hnv)

- [設定和即時移轉](#bkmk_live)

- [在傳送封包的 MAC 位址使用](#bkmk_mac)

- [管理將小組設定](#bkmk_manage)

## <a name="bkmk_over"></a>設定概觀

集合是替代的 NIC 小組解決方案，包括 HYPER-V 和軟體定義網路的環境中，您可以使用\(SDN\) Windows Server 2016 中的堆疊。 設定會將部分 NIC 小組功能整合到 HYPER-V 虛擬交換器。

設定可讓您一到八個實體乙太網路介面卡之間到一或多個以軟體為基礎的虛擬網路介面卡群組。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。

集合成員的網路介面卡必須全部安裝在相同的實體 HYPER-V 主機來放置在一個小組。

> [!NOTE]
> Windows Server 2016 中的 HYPER-V 虛擬交換器只支援設定使用。 您無法部署在 Windows Server 2012 R2 中的設定。

您可以在相同的實體交換器或不同的實體交換器連接您組合的 Nic。 如果您的 Nic 連接到不同的交換器時，這兩個參數必須是相同的子網路上。

下圖說明集架構。

![設定架構](../media/RDMA-and-SET/set_architecture.jpg)

因為集合已整合至 HYPER-V 虛擬交換器，您無法使用設定於虛擬機器 (VM)。 您可以不過使用 NIC 小組內的 Vm。

如需詳細資訊，請參閱 <<c0> [ 虛擬機器 (Vm) 中的 NIC 小組](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms)。

此外，集合架構不會公開小組介面。 相反地，您必須設定 HYPER-V 虛擬交換器連接埠。

## <a name="bkmk_avail"></a>設定可用性

集合是適用於所有 Windows Server 2016 的版本，包括 HYPER-V 和 SDN 堆疊。 此外，您可以使用 Windows PowerShell 命令和遠端桌面連線從遠端電腦執行這些工具受到支援的用戶端作業系統管理組。

## <a name="bkmk_nics"></a>支援的 Nic 設定

您可以使用任何已通過 Windows 硬體限定性條件和標誌的乙太網路 NIC \(WHQL\)測試集小組中 Windows Server 2016。 設定時就需要將小組成員的所有網路介面卡必須都是相同\(亦即，相同製造商，相同模型，相同韌體和驅動程式\)。 設定支援一到八個網路介面卡，在小組之間。
  
## <a name="bkmk_compat"></a>設定與 Windows 伺服器網路技術的相容性

設定適用於 Windows Server 2016 中的下列網路技術。

- 資料中心橋接\(DCB\)
  
- HYPER-V 網路虛擬化-內華達州拉斯維加斯 GRE 與 VxLAN 可同時支援 Windows Server 2016。  
- 接收端總和檢查碼卸載\(IPv4，IPv6，TCP\) -如果任何一組小組成員支援支援這些。

- 遠端直接記憶體存取\(RDMA\)

- 單一根目錄 I/O 虛擬化\(SR-IOV\)

- 傳輸端總和檢查碼卸載\(IPv4，IPv6，TCP\) -支援這些如果所有設定小組成員的支援。

- 虛擬機器佇列\(VMQ\)

- 虛擬接收端調整\(RSS\)

無法與下列網路技術的 Windows Server 2016 相容組。

- 802.1x 驗證。 802.1x 可延伸驗證通訊協定\(EAP\)封包會自動卸除 Hyper-v\-V 集案例中的虛擬交換器。
 
- IPsec 工作卸載\(IPsecTO\)。 這是舊的技術，不支援大部分的網路介面卡，而其中存在，它預設會停用。

- 使用 QoS \(pacer.exe\)主應用程式或原生作業系統中。 這些 QoS 案例不是超\-V 案例中，因此技術沒有交集。 此外，QoS 是可用的但預設並未啟用-您必須刻意讓 QoS。

- 接收端聯合\(RSC\)。 Hyper-v 會自動停用 RSC\-V 虛擬交換器。

- 接收端調整\(RSS\)。 HYPER-V 使用 VMQ 和 VMMQ 的佇列，因為當您建立虛擬交換器時，會一律停用 RSS。

- TCP Chimney 卸載。 預設會停用這項技術。

- 虛擬機器 QoS \(VM QoS\)。 VM QoS 是可用的但預設為停用。 如果您在設定的環境中設定 VM 的 QoS，QoS 設定將會造成無法預期的結果。

## <a name="bkmk_modes"></a>設定模式和設定

不同於 NIC 小組，當您建立將小組設定，您無法設定小組名稱。 此外，使用待命的配接器支援 NIC 小組，但不是支援集合中。 當您部署設定時，所有網路介面卡而且沒有任何處於待命模式。

NIC 小組與設定之間的另一個主要差異是，NIC 小組提供選擇的三種不同的小組模式，而集僅支援**交換器獨立**模式。 交換器獨立模式中，交換器或設定小組成員所連接的交換器不知道組小組的目前狀態，並不會決定如何分散網路流量來設定小組成員-相反地，將小組散發輸入的網路設定小組成員之間的流量。

當您建立新的組小組時，您必須設定下列 team 屬性。

- 成員介面卡

- 負載平衡模式

### <a name="member-adapters"></a>成員介面卡

當您建立將小組設定時，您必須指定為設定小組成員介面卡繫結至 HYPER-V 虛擬交換器的最多八個相同的網路介面卡。

### <a name="load-balancing-mode"></a>負載平衡模式

集的選項可讓您小組負載平衡分配模式都**HYPER-V 通訊埠**並**動態**。

**Hyper-V Port**

Vm 會連線到 HYPER-V 虛擬交換器上的連接埠。 使用 HYPER-V 通訊埠模式時組小組，HYPER-V 虛擬交換器連接埠和相關聯的 MAC 位址來分割集小組成員之間的網路流量。

> [!NOTE]
> 當您使用集合 Packet Direct，小組模式搭配**交換器獨立**和負載平衡模式**HYPER-V 通訊埠**所需。

相鄰的交換器一律會在指定的連接埠上看到特定的 MAC 位址，因為參數輸入負責將負載分散 （的流量從交換器到主機） 的 MAC 位址所在的連接埠。 這是特別有用的虛擬機器佇列 (Vmq) 使用時，因為佇列可以放在特定位置的流量應該抵達的 NIC。

不過，如果主機有只有少數的 Vm，此模式可能不細微，以達到平衡良好的分佈。 此模式也一律會限制單一 VM （亦即，從單一的交換器連接埠的流量） 是在單一介面上的 可用的頻寬。

**Dynamic**

此負載平衡模式提供下列優點。

- 輸出的負載會散發的 TCP 連接埠和 IP 位址的雜湊為基礎。  動態模式也重新平衡即時負載，讓指定的輸出流程可以設定小組成員之間來回移動。

- 輸入的負載會散發中的 HYPER-V 連接埠模式相同的方式。

在此模式中的輸出負載動態地平衡根據 flowlets 的概念。 就像人類的語音有字詞和句子結尾處的自然中斷，TCP 流量 （TCP 通訊資料流） 也會有自然發生中斷。 兩個這類符號之間 TCP 流量的部分被指 flowlet。

當動態模式演算法偵測到，flowlet 界限發現-例如長度足夠的中斷發生時在 TCP 流程-演算法會自動重新平衡到另一個小組成員視流程。  在某些罕見的情況下，演算法可能也會定期重新平衡不包含任何 flowlets 的流程。 因為這個緣故，TCP 流程和小組成員之間的親和性可以變更在任何時候，動態載入平衡演算法處理以平衡小組成員的工作負載。

## <a name="bkmk_vmq"></a>設定和虛擬機器佇列 (Vmq)

VMQ 和組良好搭配運作，以及每當您使用 HYPER-V 和設定，您應該啟用 VMQ。

> [!NOTE]
> 設定 永遠顯示跨所有集可供使用佇列的總數，小組成員。 在 NIC 小組，這稱為總和的佇列模式。

大部分的網路介面卡都可用於任一接收端調整的佇列\(RSS\)或 VMQ，但不要同時在相同的時間。
  
VMQ 的某些設定看起來似乎設定 RSS 佇列，但其實是 RSS 和 VMQ 的使用，視哪一項功能目前正在使用中的一般佇列上的設定。 每個 NIC 具有，其進階的內容中的值`*RssBaseProcNumber`和`*MaxRssProcessors`。

以下是一些 VMQ 設定提供更好的系統效能。

- 在理想情況下每個 NIC 應有`*RssBaseProcNumber`設為偶數，大於或等於二 （2)。 這是因為第一個實體處理器核心 0 \(0 和 1 的邏輯處理器\)，通常會負責大部分的系統處理，因此網路處理應該來操縱離開此實體的處理器。 

>[!NOTE]
>某些機器架構不需要每個實體處理器，兩個邏輯處理器，因此針對這類機器基底的處理器應該大於或等於 1。 如果在有疑問，假設您的主機會使用 2 個邏輯處理器，每個實體處理器架構。

- 小組成員的處理器應該是，它會實際、 非重疊的。 例如，在 4 核心主機\(8 個邏輯處理器\)與的 2 個 10 gbps Nic 小組，您可以設定第一個使用基底的 2 的處理器，並使用 4 個核心，第二個會設定為使用基底處理器 6，並使用 2 個核心。

## <a name="bkmk_hnv"></a>設定與 HYPER-V 網路虛擬化\(HNV\)

集合是與 HYPER-V 網路虛擬化在 Windows Server 2016 完全相容。 HNV 管理系統會提供資訊來設定驅動程式，可讓設定為分散網路流量負載以最適合用於 HNV 流量的方式。
  
## <a name="bkmk_live"></a>設定和即時移轉

Windows Server 2016 中，便支援即時移轉。

## <a name="bkmk_mac"></a>在傳送封包的 MAC 位址使用

當您設定集小組使用動態負載分佈，來自單一來源的封包\(例如單一 VM\)會同時分散到多個小組成員。 

若要防止交換器弄混了，而且若要防止 MAC flapping 警示，設定取代針對以外的親和的小組成員的小組成員所傳輸的框架上不同的 MAC 位址中的來源 MAC 位址。 因為這個緣故，每位小組成員使用不同的 MAC 位址，並後，才發生失敗，系統會防止 MAC 位址衝突。

小組軟體集主要 NIC 上偵測到失敗時，就會開始在選擇要做為暫存的親和的小組成員的小組成員使用 VM 的 MAC 位址\(亦即，現在會於的選項顯示為 VM 的其中一個介面\)。

這項變更僅適用於即將要傳送的 VM 自己的 MAC 位址的 VM 的親和的小組成員在作為其來源 MAC 位址的流量。 其他流量會繼續與任何來源 MAC 位址，它會使用故障前一併傳送。

以下是描述集小組 MAC 位址取代行為，根據小組的設定方式的清單：

- 在 HYPER-V 通訊埠散發的交換器獨立模式

    - 每個 vmSwitch 連接埠會與小組成員
  
    - 每個封包會傳送連接埠相似化的小組成員  
  
    - 在不完成任何來源 MAC 取代  
  
- 在 交換器獨立模式使用動態通訊群組
  
    - 每個 vmSwitch 連接埠會與小組成員  
  
    - ARP/NS 的所有封包傳送連接埠相似化的小組成員上  
  
    - 上是相似的小組成員的小組成員傳送的封包會有任何來源 MAC 位址取代完成  
  
    - 必須完成的來源 MAC 位址取代以外的親和的小組成員的小組成員傳送的封包。  
  
## <a name="bkmk_manage"></a>管理將小組設定

建議您使用 System Center Virtual Machine Manager \(VMM\)管理組小組，不過您可以也使用 Windows PowerShell 來管理集合。 下列各節提供 Windows PowerShell 命令，您可以使用管理組。

如需如何建立一組小組使用 VMM，請參閱 System Center VMM 程式庫 > 主題中的"的邏輯交換器設定 」 一節[建立邏輯交換器](https://docs.microsoft.com/system-center/vmm/network-switch)。
  
### <a name="create-a-set-team"></a>建立將小組設定

您必須使用建立 HYPER-V 虛擬交換器的同時建立設定團隊**New-vmswitch** Windows PowerShell 命令。

當您建立 HYPER-V 虛擬交換器時，您必須包含新**EnableEmbeddedTeaming**命令語法中的參數。 在下列範例中，HYPER-V 交換器的名稱為**TeamedvSwitch**與內嵌小組和兩個初始小組建立的成員。
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
**EnableEmbeddedTeaming**參數會假設為 Windows PowerShell 時的引數**NetAdapterName**是陣列的 Nic，而不是單一 nic。 如此一來，您也可以以下列方式修改前一個命令。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

如果您想要建立能夠設定切換一名小組成員，讓您可以新增小組成員在稍後的時間，則您必須使用 EnableEmbeddedTeaming 參數。

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>新增或移除設定小組成員

**組 VMSwitchTeam**命令包含**NetAdapterName**選項。 若要變更小組成員將小組設定中的，輸入所需的清單之後的小組成員**NetAdapterName**選項。 如果**TeamedvSwitch**最初建立 NIC 1 和 NIC 2，則下列範例命令會刪除設定小組成員 」 NIC 2 」，並加入新的設定小組成員 」 NIC 3"。
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>移除將小組設定

您可以移除集小組只是藉由移除 HYPER-V 虛擬交換器，其中包含將小組設定。  使用主題[Remove-vmswitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch)如需如何移除 HYPER-V 虛擬交換器的詳細資訊。 下列範例會移除名為的虛擬交換器**SETvSwitch**。

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>變更集小組的負載分配演算法

**組 VMSwitchTeam** cmdlet 具有**LoadBalancingAlgorithm**選項。 此選項會使用其中兩個可能值：**HyperVPort**或是**動態**。 若要設定或變更交換器內嵌小組的負載分配演算法，使用此選項。 

在下列範例中，具名 VMSwitchTeam **TeamedvSwitch**會使用**動態**負載平衡演算法。  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>相似化到實體的小組成員的虛擬介面

設定可讓您建立的虛擬介面之間的相似性\(亦即，HYPER-V 虛擬交換器連接埠\)，另一個小組中的實體 Nic。 

例如，如果您建立兩個主機 Vnic SMB\-直接節所述[使用 SET 和 RDMA Vnic 建立 HYPER-V 虛擬交換器](#bkmk_set-rdma)，您可以確保兩個 Vnic，使用不同的小組成員。 

加入該區段中的指令碼，您可以使用下列 Windows PowerShell 命令。

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

本主題會檢查中的區段 4.2.5 深入[Windows Server 2016 NIC 和交換器內嵌小組使用者指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)。
