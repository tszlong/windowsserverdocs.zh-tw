---
title: NIC 小組
description: 在本主題中，我們提供您的網路介面卡 (NIC) 小組概觀 Windows Server 2016 中。 NIC 小組可讓您將介於 1 到 32 到一或多個以軟體為基礎的虛擬網路介面卡的實體 Ethernet 網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: cf58956ead8e8a47b8ec6d189bf23e5c576d5f15
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812190"
---
# <a name="nic-teaming"></a>NIC 小組

>適用於：Windows Server （半年通道），Windows Server 2016

在本主題中，我們提供您的網路介面卡 (NIC) 小組概觀 Windows Server 2016 中。 NIC 小組可讓您將介於 1 到 32 到一或多個以軟體為基礎的虛擬網路介面卡的實體 Ethernet 網路介面卡。 這些虛擬網路介面卡可在網路介面卡故障時，提供快速的效能與容錯。  
  
> [!IMPORTANT]
> 您必須在相同的實體主機電腦中安裝 NIC 小組成員網路介面卡。 

> [!TIP]  
> NIC 小組，其中包含只有一個網路介面卡無法提供負載平衡與容錯移轉。 不過，使用一個網路介面卡，您可以使用 NIC 小組的網路流量的區隔時，您也正在使用虛擬區域網路 (Vlan)。  
  
當您設定網路介面卡組成一個 NIC 小組時，它們會連接到 NIC 小組解決方案的通用核心，然後提供一個或多個虛擬介面卡 （也稱為 team Nic [tNICs] 或小組介面） 向作業系統。 

由於 Windows Server 2016 支援每個小組的最多可包含 32 個小組介面，有各種不同的演算法，將各個 Nic 之間的輸出流量 （負載）。  下圖說明 NIC 小組與多個 tNICs。  
  
![NIC 小組與多個 tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
此外，您可以連接您組合的 Nic 相同的交換器或不同的交換器。 如果您的 Nic 連接到不同的交換器時，這兩個參數必須是相同的子網路上。  
  
## <a name="availability"></a>可用性  
NIC 小組可在所有版本的 Windows Server 2016。 您可以使用各種工具來管理電腦執行用戶端作業系統，例如從的 NIC 小組: • Windows PowerShell cmdlet • 遠端桌面 • 遠端伺服器管理工具  
  
## <a name="supported-and-unsupported-nics"></a>支援和不支援的 Nic   
您可以使用任何已通過 Windows 硬體限定性條件與標誌測試 （WHQL 測試），在 Windows Server 2016 中的 NIC 小組的乙太網路 NIC。  
  
您不可以將下列 Nic 放在 NIC 小組：
  
-   會公開為 Nic 主機分割區中的 HYPER-V 虛擬交換器連接埠的 HYPER-V 虛擬網路介面卡。  
  
    > [!IMPORTANT]  
    > 請勿將 HYPER-V 虛擬 Nic 小組中的主機磁碟分割 (Vnic) 中公開。 不支援任何組態中的主機磁碟分割內的 Vnic 小組。 如果發生網路失敗，則小組 Vnic 的嘗試可能會導致完全中斷的通訊。  
  
-   核心偵錯網路介面 (KDNIC) 卡。  
  
-   用於網路開機的 Nic。  
  
-   使用乙太網路，例如 WWAN，WLAN/Wi-fi、 藍笌，以及 Infiniband，包括透過 Infiniband (IPoIB) Nic 的網際網路通訊協定以外的技術的 Nic。  
  
## <a name="compatibility"></a>相容性  
NIC 小組適用於所有網路技術的 Windows Server 2016 有下列例外狀況。  
  
-   **單一根目錄 I/O 虛擬化 (SR-IOV)** 。 對於 SR-IOV，資料會直接傳遞到 NIC 而不會通過網路堆疊 （在主機作業系統，在虛擬化的情況下）。 因此，不可能的 NIC 小組，來檢查，或將資料重新導向至另一個小組中的路徑。  
  
-   **原生的主機服務品質 (QoS)** 。 當您將原生或主機系統上的 QoS 原則和這些原則叫用最小頻寬限制，NIC 小組的整體輸送量小於它在沒有頻寬原則。  
  
-   **TCP Chimney**。 不支援 TCP Chimney NIC 小組，因為 TCP Chimney 卸載整個網路堆疊直接至 nic。  
  
-   **802.1x 驗證**。 您不應該使用與 「 NIC 小組 802.1x 驗證，因為有些參數不允許 802.1 X 驗證與 NIC 小組上相同的連接埠的組態。  
  
若要了解如何使用 NIC 小組內的 HYPER-V 主機執行的虛擬機器 (Vm)，請參閱[主機電腦或 VM 上建立新的 NIC 小組](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)。
  
## <a name="virtual-machine-queues-vmqs"></a>虛擬機器佇列 (Vmq)  

Vmq 是 NIC 功能可針對每個 VM 配置的佇列。  每當您有已啟用; HYPER-V您也必須啟用 VMQ。 在 Windows Server 2016 中，Vmq 使用 NIC 交換器 vPorts 與指派給 vPort 單一佇列提供相同的功能。 

根據參數設定模式和負載分配演算法，NIC 小組提供小組 （最小佇列模式） 中的任何配接器所提供及支援佇列的最小數目或可用的佇列的總數所有小組（加總的佇列模式） 的成員。  

如果小組會在 交換器獨立小組模式中，您將負載分配設定為 HYPER-V 通訊埠模式或動態模式的報告佇列數目會是可從 小組成員 （加總的佇列模式） 的所有佇列的總和。 否則，報告的佇列數目是佇列支援小組 （最小佇列模式） 的任何成員的最小數目。

原因如下：  
  
-   當交換器獨立小組永遠會為 HYPER-V 通訊埠模式或動態模式中的 HYPER-V 交換器通訊埠 (VM) 的流量到達相同的小組成員。 主機可以預測/控制哪個成員會接收特定 vm 的流量因此 NIC 小組可以更縝密的相關特定小組成員上配置的 VMQ 佇列。 NIC 小組，使用 HYPER-V 交換器中，為 vm 設定 VMQ 上一個小組成員, 和知道的輸入的流量都會抵達該佇列。  
  
-   當小組處於任何交換器相依模式 （靜態小組或 LACP 小組），小組已連線到交換器會控制輸入的流量分佈。 無法預測的主機 NIC 小組的軟體，讓小組成員取得輸入的流量的 vm，而且可能是，參數會將流量分配 VM 的所有團隊成員。 因為結果的 NIC 小組的軟體，使用 HYPER-V 交換器中，程式佇列上的 VM 每位團隊成員，不只是其中一個團隊成員。  
  
-   當小組在 交換器獨立模式和使用位址雜湊負載平衡，輸入的流量一律是上一個 NIC （主要小組成員） – 全都在一個小組成員。 其他小組成員未處理的輸入流量，因為它們取得編寫使用相同的佇列做為主要的成員，如果主要成員失敗時，任何其他小組成員可以用來收取輸入流量，而且佇列便已存在。  

- 大部分的 Nic 有佇列用於接收端調整 (RSS) 或 VMQ，但不是在相同的時間。 某些 VMQ 設定看起來似乎設定 RSS 佇列，但是 RSS 和 VMQ 的使用，視哪一項功能目前正在使用中的一般佇列上設定。 每個 NIC 具有，在進階的內容中，值為 * RssBaseProcNumber 和\*將 MaxRssProcessors。 以下是一些 VMQ 設定提供更好的系統效能。  
  
-   在理想情況下，每個 NIC 應有 * RssBaseProcNumber 會設為偶數，大於或等於二 （2)。 第一個實體處理器，核心 0 （邏輯處理器 0 和 1），通常會大部分的系統處理，因此網路處理應該操縱離開此實體的處理器。 某些機器架構不需要兩個邏輯處理器，每個實體處理器，因此針對這類機器中，基底的處理器應該大於或等於 1。 如果不確定假設您的主機會使用 2 個邏輯處理器，每個實體處理器架構。  
  
-   如果小組是加總的佇列模式應該是小組成員的處理器，非重疊。 例如，在具有 2 個 10 gbps Nic 小組的 4 核心主機 （8 個邏輯處理器），您可以設定第一個使用基底 2 個處理器，並使用 4 個核心，第二個會設定為使用基底處理器 6，並使用 2 個核心。  
  
-   如果小組在最小佇列模式小組成員使用的處理器設定必須相同。  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Hyper-V 網路虛擬化 (HNV)  
NIC 小組可完全相容與 HYPER-V 網路虛擬化 (HNV)。  HNV 管理系統提供的 NIC 小組的驅動程式，可讓 NIC 小組來分配方式最佳化 HNV 流量的負載資訊。  
  
## <a name="live-migration"></a>即時移轉  
Vm 中 NIC 小組不會影響即時移轉。 進行即時移轉的相同規則存在，在 VM 中設定 NIC 小組。  


## <a name="virtual-local-area-networks-vlans"></a>虛擬區域網路 (Vlan)
當您使用 NIC 小組時，建立多個小組介面可讓主應用程式連接至不同的 Vlan，在相同的時間。 設定您環境中使用下列指導方針：
  
- 在您啟用 NIC 小組之前，設定連接至小組的主機，以使用 （混合） 的主幹模式中的實體交換器連接埠。 實體交換器應該將所有流量都傳遞至主應用程式，而不需要修改的流量篩選。  

- 請勿 Nic 上設定 VLAN 的篩選器，使用 進階屬性設定的 NIC。 讓 NIC 小組的軟體，或 HYPER-V 虛擬交換器 （如果有的話） 來執行 VLAN 的篩選。  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>使用虛擬區域網路中 VM 的 NIC 小組  
當小組連接至 HYPER-V 虛擬交換器時，就必須完成所有的 VLAN 隔離在 HYPER-V 虛擬交換器，而不是 NIC 小組中。  

規劃使用 Vlan，在 VM 中設定與 NIC 小組，請使用下列方針：
  
-   在 VM 中支援多個 Vlan 的慣用的方法是將 VM 設定與 HYPER-V 虛擬交換器上的多個連接埠及關聯的 VLAN 的每個連接埠。 永遠不會小組在 VM 中的這些連接埠，因為這麼做會導致網路通訊問題。  

-   如果 VM 有多個 SR-IOV 虛擬函式 (VFs)，請確定它們在之前其小組中的 VM 位於相同的 VLAN。 您可輕鬆地設定要在不同的 Vlan 上不同 VFs，而且會產生網路通訊問題。  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>管理網路介面與虛擬區域網路 
如果您必須有公開到客體作業系統的多個 VLAN，請考慮重新命名以釐清 VLAN 指派給介面的乙太網路介面。 比方說，如果您將建立關聯**乙太網路**具有 VLAN 12 的介面和**乙太網路 2**介面具有 VLAN 48、 重新命名乙太網路的介面來**EthernetVLAN12**和其他要**EthernetVLAN48**。 

使用 Windows PowerShell 命令來重新命名介面**重新命名 NetAdapter**或藉由執行下列程序：
  
1.  在 伺服器管理員 中，在**屬性**您想要重新命名網路介面卡的情況下，按一下 網路介面卡名稱右邊的連結。 
  
2.  以滑鼠右鍵按一下您想要重新命名和選取的網路介面卡**重新命名**。  
  
3.  輸入新的網路介面卡的名稱，然後按 ENTER。  


## <a name="virtual-machines-vms"></a>虛擬機器 (Vm)

如果您想要使用在 VM 中的 NIC 小組，您必須連接至外部 HYPER-V 虛擬交換器只的 VM 中的虛擬網路介面卡。 這樣做可讓 VM 可以承受網路連線，即使在當時的情況中，當其中一個實體網路介面卡連線到一個虛擬交換器失敗或中斷連接。 連線到內部或私人的 HYPER-V 虛擬交換器的虛擬網路介面卡不能在一個小組，也就是，但 vm 的網路失敗時，連接到的交換器。  
  
Windows Server 2016 中的 NIC 小組在 Vm 中支援使用兩個成員的小組。 您可以建立較大規模的小組，但不支援較大規模的小組。 每位團隊成員必須連接到不同外部 HYPER-V 虛擬交換器，以及 VM 的網路介面必須設定為允許小組。

  
如果您要在 VM 中設定 NIC 小組，您必須選取**小組模式**的_交換器獨立_並**負載平衡模式**的_位址雜湊_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>SR 支援 SR-IOV 的網路介面卡  
NIC 小組中，或在 HYPER-V 主機不能保護 SR-IOV 流量，因為它不會進出 HYPER-V 交換器。  與 VM NIC 小組 選項中，您可以設定兩個外部的 HYPER-V 虛擬交換器，每個連接至其本身的 SR-IOV 支援 SR-IOV nic。  
  
![NIC 小組與 SR 支援 SR-IOV 的網路介面卡](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
每個 VM 可以有虛擬函式 (VF) 從一個或兩個 SR-IOV Nic 和 NIC 中斷連接，從主要 VF 容錯移轉至備份配接器 (VF) 發生。 或者，VM 可能會有 VF 從一個 NIC 和非 VF vmNIC 連線到另一個虛擬交換器。 如果 VF 相關聯的 NIC 中斷連接，流量就可以容錯移轉到另一個交換器沒有連線中斷。  
  
因為 VM 中的 Nic 之間的容錯移轉可能會導致其他 vmNIC 的 MAC 位址傳送流量，與使用 NIC 小組的虛擬機器相關聯的每個 HYPER-V 虛擬交換器連接埠必須設定為允許小組。 


## <a name="related-topics"></a>相關主題

- [NIC 小組 MAC 位址使用和管理](NIC-Teaming-MAC-Address-Use-and-Management.md):當您設定 NIC 小組與交換器獨立模式和位址雜湊或動態配置資源負載分配時，小組所使用的媒體存取控制 (MAC) 位址的輸出流量的主要 NIC 小組成員。 主要的 NIC 小組成員會將作業系統從一組初始的小組成員所選取的網路介面卡。

- [NIC 小組設定](nic-teaming-settings.md):本主題中，我們會提供您的 NIC 小組 」 屬性，例如小組的概觀，以及負載平衡模式。 我們也提供您關於待命配接器設定和主要小組介面屬性的詳細資料。 如果您有至少兩個網路介面卡在 NIC 小組時，您不需要指定容錯移轉的待命介面卡。
  
- [在主機電腦或 VM 上建立新的 NIC 小組](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md):本主題中，您會建立新的 NIC 小組，在主機電腦上或在執行 Windows Server 2016 的 HYPER-V 虛擬機器 (VM)。

- [疑難排解 NIC 小組](Troubleshooting-NIC-Teaming.md):本主題中，我們會討論如何疑難排解 「 NIC 小組，例如硬體、 實體交換器 securities 和停用或啟用使用 Windows PowerShell 的網路介面卡。 
 
