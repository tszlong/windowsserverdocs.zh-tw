---
title: 規劃的 Windows Server 中的 HYPER-V 網路功能
description: 說明什麼所需的基本網路在 HYPER-V 中，以及提供指示的連結
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 83c014b917566a78796d061dd88962966ec0c9ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829869"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>規劃的 Windows Server 中的 HYPER-V 網路功能

>適用於：Microsoft HYPER-V Server 2016、windows Server 2016 中，Microsoft HYPER-V Server 2019，Windows Server 2019
  
在 HYPER-V 中的網路功能的基本了解可協助您規劃網路服務的虛擬機器。 使用即時移轉時，並使用 HYPER-V 與其他伺服器功能和角色時，本文也涵蓋網路功能的一些考量。  
  
## <a name="hyper-v-networking-basics"></a>HYPER-V 網路基本概念  
在 HYPER-V 中的基本網路功能都相當簡單。 它會使用兩個部分-虛擬交換器和虛擬網路介面卡。 您必須至少其中每一個來建立虛擬機器的網路。 虛擬交換器會連接到任何乙太網路為基礎的網路。 虛擬網路介面卡會連接到虛擬交換器，因此可以使用網路的虛擬機器上的連接埠。  
  
建立基本的網路功能的最簡單方式是建立虛擬交換器，當您安裝 HYPER-V。 然後，當您建立虛擬機器時，您可以將它連線到交換器。 自動連線到交換器新增到虛擬機器的虛擬網路介面卡。 如需相關指示，請參閱 <<c0> [ 建立 HYPER-V 虛擬機器的虛擬交換器](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。  
  
若要處理不同類型的網路功能，您可以新增虛擬交換器和虛擬網路介面卡。 所有參數都是 HYPER-V 主機的一部分，但每個虛擬網路介面卡屬於只有一部虛擬機器。  
  
虛擬交換器的軟體式 layer-2 乙太網路交換器。 它提供內建功能監視、 控制，並將流量，以及安全性和診斷。  您可以新增一組內建功能來安裝外掛程式，也稱為*延伸模組*。 這些都是從獨立軟體廠商。 如需有關交換器和擴充功能的詳細資訊，請參閱 < [HYPER-V 虛擬交換器](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
### <a name="switch-and-network-adapter-choices"></a>交換器和網路介面卡選項  
HYPER-V 提供三種類型的虛擬交換器和兩種類型的虛擬網路介面卡。 您會選擇每個您想要建立哪一個。 您可以使用 HYPER-V 管理員] 或 [Windows PowerShell 的 HYPER-V 模組，來建立和管理虛擬交換器和虛擬網路介面卡。 某些進階的網路功能，例如延伸連接埠存取控制清單 (Acl)，可以只可使用 cmdlet 來管理 HYPER-V 模組中。  
  
建立好之後，您可以進行一些變更至虛擬交換器或虛擬網路介面卡。 比方說，就可以將現有的參數變更為不同的類型，但這麼做會影響連接到該交換器的所有虛擬機器的網路功能。  因此，您可能不會這麼做，除非您發生錯誤或需要進行某些測試。 另舉一例，您可以連接至不同的交換器，如果您想要連接到不同的網路，您可能會執行的虛擬網路介面卡。 但是，您無法從一種型別到另一個來變更虛擬網路介面卡。 而不是變更的類型，您會新增另一個虛擬網路介面卡，並選擇適當的型別。  
  
虛擬交換器類型為：  
  
-   **外部虛擬交換器**-連線到有線的實體網路繫結至實體網路介面卡。  
  
-   **內部虛擬交換器**-連線到可供只執行虛擬機器的主機上有虛擬交換器，以及主機和虛擬機器之間的網路。  
  
-   **私人虛擬交換器**-連線到網路，所以只可供與虛擬交換器，但不會提供主機和虛擬機器之間的網路在主機上執行的虛擬機器。  
  
虛擬網路介面卡類型包括：  
  
-   **HYPER-V 特定網路介面卡**-適用於第 1 代和第 2 代虛擬機器。 它專為 HYPER-V，而且必須包含 HYPER-V integration services 中的驅動程式。 這種類型的更快的網路介面卡，除非您需要進行網路開機，或執行不受支援的客體作業系統是建議的選擇。 必要的驅動程式僅供支援的客體作業系統。 請注意，在 HYPER-V 管理員和網路功能的 cmdlet，此類型只是所謂的網路介面卡。  
  
-   **傳統網路介面卡**-僅適用於第 1 代虛擬機器。 模擬 Intel 21140 型 PCI 快速乙太網路介面卡，而且可用來開機到網路讓您可以從 Windows 部署服務等服務來安裝作業系統。  
  
## <a name="hyper-v-networking-and-related-technologies"></a>HYPER-V 網路功能及相關的技術  
最新的 Windows Server 版本導入了可讓您更多的選項設定為 HYPER-V 網路功能的改善。 比方說，Windows Server 2012 引進了支援的聚合式網路。 這可讓您透過一個外部虛擬交換器的網路流量路由傳送。 Windows Server 2016 建立這項藉由繫結至 HYPER-V 虛擬交換器的網路介面卡上使用遠端直接記憶體存取 (RDMA)。 使用或不含 Switch Embedded Teaming (SET)，您可以使用此設定。 如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取&#40;RDMA&#41;和交換器內嵌小組&#40;設定&#41;</c0>](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
某些功能依賴特定的網路組態，或在特定的組態下，處理得比較好。 計劃，或更新您的網路基礎結構時，請考慮這些選項。  
  
**容錯移轉叢集**-最佳的作法，以隔離叢集流量，並使用 HYPER-V 的服務品質 (QoS) 是在虛擬交換器上。 如需詳細資訊，請參閱[HYPER-V 叢集的網路建議](https://technet.microsoft.com/library/dn550728.aspx)  
  
**即時移轉**-使用效能選項，以減少網路和 CPU 使用量和即時移轉所花費的時間。 如需相關指示，請參閱 <<c0> [ 設定即時移轉，而不容錯移轉叢集的主機](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。  
  
**儲存空間直接存取**-這項功能會依賴 SMB3.0 網路通訊協定和 RDMA。 如需詳細資訊，請參閱 <<c0> [ 儲存空間直接存取 Windows Server 2016 中](../../../storage/storage-spaces/storage-spaces-direct-overview.md)。