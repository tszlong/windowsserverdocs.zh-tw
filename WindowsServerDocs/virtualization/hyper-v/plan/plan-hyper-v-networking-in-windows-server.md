---
title: 規劃 Windows Server 中的 Hyper-v 網路功能
description: 說明 Hyper-v 中基本網路功能的需求，並提供指示的連結
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f09bc82d0dd47d3393dd05dcf03913db11e4c335
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392512"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>規劃 Windows Server 中的 Hyper-v 網路功能

>適用於：Microsoft Hyper-v Server 2016、Windows Server 2016、Microsoft Hyper-v Server 2019、Windows Server 2019
  
Hyper-v 中網路功能的基本瞭解可協助您規劃虛擬機器的網路功能。 本文也涵蓋在使用即時移轉時，以及搭配其他伺服器功能和角色使用 Hyper-v 時的一些網路考慮。  
  
## <a name="hyper-v-networking-basics"></a>Hyper-v 網路功能基本概念  
Hyper-v 中的基本網路功能相當簡單。 它使用兩個部分-虛擬交換器和虛擬網路介面卡。 您至少需要其中一個，才能建立虛擬機器的網路功能。 虛擬交換器會連接到任何以乙太網路為基礎的網路。 虛擬網路介面卡會連接到虛擬交換器上的埠，讓虛擬機器可以使用網路。  
  
建立基本網路功能的最簡單方式，就是在安裝 Hyper-v 時建立虛擬交換器。 然後，當您建立虛擬機器時，您可以將它連線至交換器。 連接到交換器會自動將虛擬網路介面卡新增至虛擬機器。 如需指示，請參閱[為 hyper-v 虛擬機器建立虛擬交換器](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。  
  
若要處理不同類型的網路，您可以新增虛擬交換器和虛擬網路介面卡。 所有參數都是 Hyper-v 主機的一部分，但每個虛擬網路介面卡只屬於一部虛擬機器。  
  
虛擬交換器是以軟體為基礎的第2層 Ethernet 網路交換器。 它提供了內建功能，可用來監視、控制和分割流量，以及安全性和診斷。  您可以藉由安裝外掛程式（也稱為*延伸*模組），將加入至內建功能的集合。 這些是由獨立軟體廠商提供。 如需有關交換器和擴充功能的詳細資訊，請參閱[Hyper-v 虛擬交換器](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。  
  
### <a name="switch-and-network-adapter-choices"></a>交換器和網路介面卡選項  
Hyper-v 提供三種類型的虛擬交換器和兩種類型的虛擬網路介面卡。 當您建立時，您會選擇所要的每一個。 您可以使用 Hyper-v 管理員或適用于 Windows PowerShell 的 Hyper-v 模組來建立和管理虛擬交換器和虛擬網路介面卡。 某些先進的網路功能（例如擴充通訊埠存取控制清單（Acl））只能使用 Hyper-v 模組中的 Cmdlet 來管理。  
  
建立虛擬交換器或虛擬網路介面卡之後，您可以對其進行一些變更。 例如，您可以將現有的交換器變更為不同的類型，但這麼做會影響連接到該交換器之所有虛擬機器的網路功能。  因此，除非您犯了錯誤或需要測試某個專案，否則您可能不會這麼做。 另一個範例是，您可以將虛擬網路介面卡連線至不同的交換器，如果您想要連線到不同的網路，您可能會這麼做。 但是，您無法將虛擬網路介面卡從一種類型變更為另一種。 您不需要變更類型，而是新增另一個虛擬網路介面卡，然後選擇適當的類型。  
  
虛擬交換器類型如下：  
  
-   **外部虛擬交換器**-藉由系結至實體網路介面卡，連接到有線實體網路。  
  
-   **內部虛擬交換器**-連線到只能供在具有虛擬交換器之主機上執行，以及在主機與虛擬機器之間執行之虛擬機器使用的網路。  
  
-   **私用虛擬交換器**-連線至只能由在具有虛擬交換器的主機上執行的虛擬機器使用的網路，但不提供主機與虛擬機器之間的網路功能。  
  
虛擬網路介面卡類型如下：  
  
-   **Hyper-v 特有的網路介面卡**-適用于第1代和第2代虛擬機器。 它是專為 Hyper-v 所設計，而且需要包含在 Hyper-v 整合服務中的驅動程式。 這種類型的網路介面卡較快，而且是建議的選擇，除非您需要開機到網路或執行不受支援的客體作業系統。 只有支援的客體作業系統才會提供所需的驅動程式。 請注意，在 Hyper-v 管理員和網路功能 Cmdlet 中，此類型只稱為網路介面卡。  
  
-   **傳統網路介面卡**-僅適用于第1代虛擬機器。 模擬 Intel 21140 型 PCI 快速 Ethernet 介面卡，並可用來開機到網路，讓您可以從像是 Windows 部署服務的服務安裝作業系統。  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Hyper-v 網路和相關技術  
最近的 Windows Server 版本引進了改良功能，可提供您更多選項來設定 Hyper-v 的網路。 例如，Windows Server 2012 引進了對聚合式網路的支援。 這可讓您透過一個外部虛擬交換器來路由傳送網路流量。 Windows Server 2016 是藉由在系結至 Hyper-v 虛擬交換器的網路介面卡上允許遠端直接記憶體存取（RDMA）來建立。 您可以使用這項設定，不論或不搭配 Switch 內嵌小組（SET）。 如需詳細資訊，請參閱[遠端&#40;直接&#41;記憶體訪問 RDMA 和&#40;交換器&#41;內嵌小組集](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
某些功能依賴特定的網路設定，或在特定設定下執行得更好。 在規劃或更新您的網路基礎結構時，請考慮這些事項。  
  
**容錯移轉**叢集-在虛擬交換器上隔離叢集流量並使用 Hyper-v 服務品質（QoS）是最佳作法。 如需詳細資訊，請參閱[hyper-v 叢集的網路建議](https://technet.microsoft.com/library/dn550728.aspx)  
  
**即時移轉**-使用效能選項來降低網路和 CPU 使用量，以及完成即時移轉所需的時間。 如需指示，請參閱[設定不含容錯移轉叢集的即時移轉主機](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。  
  
**儲存空間直接存取**-此功能依賴 smb 3.0 網路通訊協定和 RDMA。 如需詳細資訊，請參閱[Windows Server 2016 中的儲存空間直接存取](../../../storage/storage-spaces/storage-spaces-direct-overview.md)。