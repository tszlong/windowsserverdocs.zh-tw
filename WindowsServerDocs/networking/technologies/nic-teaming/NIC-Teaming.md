---
title: NIC 小組
description: 在本主題中，我們將概述 Windows Server 2016 中的網路介面卡（NIC）小組。 NIC 小組可讓您將一和32實體 Ethernet 網路介面卡分成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: lizross
author: eross-msft
ms.date: 09/10/2018
ms.openlocfilehash: f4d9dd20d626f998bee0a8414c281cd27b2d3dbb
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316439"
---
# <a name="nic-teaming"></a>NIC 小組

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，我們將概述 Windows Server 2016 中的網路介面卡（NIC）小組。 NIC 小組可讓您將一和32實體 Ethernet 網路介面卡分成一或多個以軟體為基礎的虛擬網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。  
  
> [!IMPORTANT]
> 您必須在相同的實體主機電腦中安裝 NIC 小組成員網路介面卡。 

> [!TIP]  
> 僅包含一個網路介面卡的 NIC 小組無法提供負載平衡和容錯移轉。 不過，使用一個網路介面卡，當您同時使用虛擬區域網路（Vlan）時，您可以使用 NIC 小組來分隔網路流量。  
  
當您將網路介面卡設定為 NIC 小組時，它們會連線到 NIC 小組解決方案通用核心，然後向作業系統呈現一或多個虛擬配接器（也稱為小組 Nic [tNICs] 或小組介面）。 

由於 Windows Server 2016 支援每個小組最多32個小組介面，因此有各種不同的演算法可在 Nic 之間分散輸出流量（載入）。  下圖描述具有多個 tNICs 的 NIC 小組。  
  
![具有多個 tNICs 的 NIC 小組](../../media/NIC-Teaming/nict_overview.jpg)  
  
此外，您也可以將組合的 Nic 連接到相同的交換器或不同的交換器。 如果您將 Nic 連接至不同的交換器，這兩個交換器必須位於相同的子網上。  
  
## <a name="availability"></a>可用性  
NIC 小組適用于所有版本的 Windows Server 2016。 您可以使用各種工具，從執行用戶端作業系統的電腦管理 NIC 小組，例如：• Windows PowerShell Cmdlet •遠端桌面•遠端伺服器管理工具  
  
## <a name="supported-and-unsupported-nics"></a>支援和不支援的 Nic   
您可以在 Windows Server 2016 的 NIC 小組中，使用已通過 Windows 硬體合格和標誌測試（WHQL 測試）的任何乙太網路 NIC。  
  
您不能將下列 Nic 放在 NIC 小組中：
  
-   Hyper-v 虛擬網路介面卡，這是在主機磁碟分割中公開為 Nic 的 Hyper-v 虛擬交換器埠。  
  
    > [!IMPORTANT]  
    > 請勿將主機分割區（Vnic）中公開的 Hyper-v 虛擬 Nic 放在小組中。 任何設定都不支援主機分割區內的 Vnic 小組。 如果發生網路失敗，嘗試進行小組 Vnic 可能會導致通訊完全中斷。  
  
-   內核調試網路介面卡（KDNIC）。  
  
-   用於網路開機的 Nic。  
  
-   使用乙太網路以外技術的 Nic （例如，WWAN、WLAN/Wi-fi、Bluetooth 和不等），包括透過不使用方式（IPoIB） Nic 的網際網路通訊協定。  
  
## <a name="compatibility"></a>相容性  
NIC 小組與 Windows Server 2016 中的所有網路技術相容，但有下列例外狀況。  
  
-   **單一根目錄 i/o 虛擬化（sr-iov）** 。 針對 SR-IOV，資料會直接傳遞至 NIC，而不會透過網路堆疊（在虛擬化的情況下）傳遞。 因此，NIC 小組不可能檢查或將資料重新導向至小組中的另一個路徑。  
  
-   **原生主機服務品質（QoS）** 。 當您在原生或主機系統上設定 QoS 原則，而這些原則叫用最小頻寬限制時，NIC 小組的整體輸送量會小於沒有頻寬原則的情況。  
  
-   **TCP 煙囪**。 NIC 小組不支援 TCP 煙囪，因為 TCP 煙囪會直接將整個網路堆疊卸載至 NIC。  
  
-   **802.1 x 驗證**。 您不應該使用 802.1 X 驗證搭配 NIC 小組，因為某些交換器不允許在相同埠上設定 802.1 X 驗證和 NIC 小組。  
  
若要瞭解如何在 Hyper-v 主機上執行的虛擬機器（Vm）中使用 NIC 小組，請參閱[在主機電腦或 VM 上建立新的 Nic 小組](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)。
  
## <a name="virtual-machine-queues-vmqs"></a>虛擬機器佇列（Vmq 數量）  

Vmq 數量是為每個 VM 配置佇列的 NIC 功能。  任何時候，當您啟用 Hyper-v 時，您也必須啟用 VMQ。 在 Windows Server 2016 中，Vmq 數量使用 NIC 交換器 vPorts 搭配指派給 vPort 的單一佇列來提供相同的功能。 

視交換器設定模式和負載分佈演算法而定，NIC 小組會以團隊中的任何介面卡（最小佇列模式）或所有小組可用的佇列總數，呈現最小數目的可用和支援佇列成員（佇列總和模式）。  

如果小組處於 [交換器獨立小組] 模式，而您將負載分佈設定為 [Hyper-v 埠模式] 或 [動態模式]，則報告的佇列數目是小組成員（佇列的總和模式）中所有可用佇列的總和。 否則，報告的佇列數目是小組的任何成員所支援的最小佇列數（最小佇列模式）。

原因如下：  
  
-   當交換器獨立的小組處於 Hyper-v 埠模式或動態模式時，Hyper-v 交換器埠（VM）的輸入流量一律會抵達相同的小組成員。 主機可以預測/控制哪一個成員接收特定 VM 的流量，因此，NIC 小組可以更仔細地瞭解哪些 VMQ 佇列要在特定小組成員上配置。 NIC 小組使用 Hyper-v 交換器時，會將虛擬機器的 VMQ 設定為精確的一個小組成員，並知道輸入流量會達到該佇列。  
  
-   當團隊處於任何切換相依模式（靜態小組或 LACP 小組）時，小組所連接的參數會控制輸入流量分佈。 主機的 NIC 小組軟體無法預測哪個團隊成員取得 VM 的輸入流量，而且可能是交換器會將 VM 的流量分散到所有小組成員。 由於 NIC 小組軟體的結果，使用 Hyper-v 交換器時，會在每個小組成員（而不只是一個小組成員）上，為 VM 進行佇列的程式。  
  
-   當小組處於切換獨立模式並使用位址雜湊負載平衡時，輸入流量一律會出現在一個 NIC （主要小組成員）上，只會在一個小組成員上。 由於其他小組成員不會處理輸入流量，因此會使用與主要成員相同的佇列來進行程式設計，因此，如果主要成員失敗，則任何其他小組成員都可以用來挑選輸入流量，而佇列也已準備就緒。  

- 大部分的 Nic 都有用於接收端調整（RSS）或 VMQ 的佇列，但不是同時使用。 某些 VMQ 設定似乎是 RSS 佇列的設定，但根據目前正在使用的功能，一般佇列上的設定是由 RSS 和 VMQ 所使用。 每個 NIC 在其 advanced 屬性中都有 * RssBaseProcNumber 和 \*MaxRssProcessors 的值。 以下是一些可提供較佳系統效能的 VMQ 設定。  
  
-   在理想的情況下，每個 NIC 都應該將 * RssBaseProcNumber 設定為大於或等於二（2）的偶數數位。 第一個實體處理器（核心0（邏輯處理器0和1））通常會執行大部分的系統處理，因此網路處理應該會脫離此實體處理器。 某些機器架構沒有每個實體處理器有兩個邏輯處理器，因此，針對這類機器，基底處理器應該大於或等於1。 如果有疑問，請假設您的主機使用每個實體處理器架構2個邏輯處理器。  
  
-   如果小組是在佇列的總和模式中，則小組成員的處理器應該不重迭。 例如，在具有2個 10Gbps Nic 之小組的4核心主機（8個邏輯處理器）中，您可以將第一個設定為使用基底處理器2，並使用4個核心;第二個設定為使用基本處理器6，並使用2個核心。  
  
-   如果小組處於最小佇列模式，則小組成員所使用的處理器集必須完全相同。  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Hyper-V 網路虛擬化 (HNV)  
NIC 小組與 Hyper-v 網路虛擬化（HNV）完全相容。  HNV 管理系統提供 NIC 小組驅動程式的資訊，可讓 NIC 小組以優化 HNV 流量的方式散發負載。  
  
## <a name="live-migration"></a>即時移轉  
Vm 中的 NIC 小組不會影響即時移轉。 無論是否在 VM 中設定 NIC 小組，都有相同的規則即時移轉。  


## <a name="virtual-local-area-networks-vlans"></a>虛擬區域網路（Vlan）
當您使用 NIC 小組時，建立多個團隊介面可以讓主機同時連接到不同的 Vlan。 使用下列指導方針來設定您的環境：
  
- 啟用 NIC 小組之前，請先設定連線到小組主機的實體交換器埠，以使用主幹（混雜）模式。 實體交換器應將所有流量傳遞至主機，以便在不修改流量的情況下進行篩選。  

- 請勿使用 [NIC] [advanced properties] 設定來設定 Nic 上的 VLAN 篩選器。 讓 NIC 小組軟體或 Hyper-v 虛擬交換器（如果有的話）執行 VLAN 篩選。  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>在 VM 中搭配使用 Vlan 與 NIC 小組  
當小組連接至 Hyper-v 虛擬交換器時，所有 VLAN 隔離都必須在 Hyper-v 虛擬交換器中完成，而不是在 NIC 小組中進行。  

請使用下列指導方針，規劃在使用 NIC 小組設定的 VM 中使用 Vlan：
  
-   在 VM 中支援多個 Vlan 的慣用方法，是在 Hyper-v 虛擬交換器上設定具有多個埠的 VM，並將每個埠與 VLAN 產生關聯。 永遠不要將這些埠小組在 VM 中，因為這樣做會造成網路通訊問題。  

-   如果 VM 有多個 SR-IOV 虛擬函式（VFs），請確定它們位於相同的 VLAN 上，然後再將它們小組在 VM 中。 您可以輕鬆地將不同的 VFs 設定在不同的 Vlan 上，這樣做會造成網路通訊問題。  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>管理網路介面和 Vlan 
如果您必須將一個以上的 VLAN 公開到客體作業系統，請考慮重新命名 Ethernet 介面，以澄清指派給介面的 VLAN。 例如，如果您將**ethernet**介面與 vlan 12 和**ethernet 2**介面關聯至 vlan 48，請將介面 Ethernet 重新命名為**EthernetVLAN12** ，並將另一個設為**EthernetVLAN48**。 

使用 Windows PowerShell 命令**get-netadapter**重新命名介面，或執行下列程式：
  
1.  在伺服器管理員中，**在您**要重新命名之網路介面卡的 [內容] 中，按一下網路介面卡名稱右邊的連結。 
  
2.  在您要重新命名的網路介面卡上按一下滑鼠右鍵，然後選取 [**重新命名**]。  
  
3.  輸入網路介面卡的新名稱，然後按 ENTER 鍵。  


## <a name="virtual-machines-vms"></a>虛擬機器（Vm）

如果您想要在 VM 中使用 NIC 小組，您必須只將 VM 中的虛擬網路介面卡連線到外部 Hyper-v 虛擬交換器。 這麼做可讓 VM 即使在其中一個連線到一個虛擬交換器的實體網路介面卡失敗或中斷連接的情況下，仍可維持網路連線能力。 連線到內部或私人 Hyper-v 虛擬交換器的虛擬網路介面卡，在其位於小組中時無法連接到交換器，且 VM 的網路功能會失敗。  
  
Windows Server 2016 中的 NIC 小組支援在 Vm 中有兩個成員的團隊。 您可以建立更大的小組，但不支援較大的小組。 每個小組成員都必須連接到不同的外部 Hyper-v 虛擬交換器，而且 VM 的網路介面必須設定為允許小組。

  
如果您要在 VM 中設定 NIC 小組，您必須選取 [_交換器獨立_] 和 [_位址雜湊_的**負載平衡模式]** 的**分組模式**。   
  
  
## <a name="sr-iov-capable-network-adapters"></a>支援 SR-IOV 的網路介面卡  
或在 Hyper-v 主機底下的 NIC 小組無法保護 SR-IOV 流量，因為它不會經過 Hyper-v 交換器。  使用 VM NIC 小組選項，您可以設定兩個外部 Hyper-v 虛擬交換器，每個都連接到它自己的支援 SR-IOV 的 NIC。  
  
![NIC 小組搭配具備 SR-IOV 功能的網路介面卡](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
每個 VM 可以有一或兩個 SR-IOV Nic 的虛擬函式（VF），而在 NIC 中斷連線時，會從主要 VF 容錯移轉至備份介面卡（VF）。 或者，VM 可能會有一個 NIC 的 VF，以及連接到另一個虛擬交換器的非 VF vmNIC。 如果與 VF 關聯的 NIC 中斷連線，流量就可以容錯移轉至另一個交換器，而不會失去連線能力。  
  
由於 VM 中的 Nic 之間的容錯移轉可能會導致流量與其他 vmNIC 的 MAC 位址一起傳送，因此，與使用 NIC 小組的 VM 相關聯的每個 Hyper-v 虛擬交換器埠都必須設定為允許小組。 


## <a name="related-topics"></a>相關主題

- [NIC 小組 MAC 位址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md)：當您使用「交換器獨立模式」和「位址雜湊」或「動態」負載分佈來設定 NIC 小組時，小組會使用連出流量的主要 NIC 小組成員的媒體存取控制（MAC）位址。 主要 NIC 小組成員是由一組初始小組成員的作業系統選取的網路介面卡。

- [Nic 小組設定](nic-teaming-settings.md)：在本主題中，我們將概述 nic 團隊的屬性，例如團隊和負載平衡模式。 我們也會提供有關待命介面卡設定和主要小組介面屬性的詳細資料。 如果您的 NIC 小組中至少有兩張網路介面卡，則不需要指定待命介面卡來容錯。
  
- 在[主機電腦或 VM 上建立新的 Nic 小組](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)：在本主題中，您會在主機電腦或執行 Windows Server 2016 的 hyper-v 虛擬機器（VM）中建立新的 nic 小組。

- 針對[nic 小組進行疑難排解](Troubleshooting-NIC-Teaming.md)：在本主題中，我們會討論針對 nic 小組進行疑難排解的方法，例如硬體、實體交換器的安全性，以及使用 Windows PowerShell 來停用或啟用網路介面卡。 
 
