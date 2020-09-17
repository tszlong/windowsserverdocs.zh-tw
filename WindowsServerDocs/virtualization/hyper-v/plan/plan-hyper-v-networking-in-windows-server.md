---
title: 規劃 Windows Server 中的 Hyper-v 網路功能
description: 描述 Hyper-v 中的基本網路所需的內容，並提供指示的連結
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: e36ebb6d90dcb4fc05e05c135a49f84e850249b2
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745933"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>規劃 Windows Server 中的 Hyper-v 網路功能

>適用于： Microsoft Hyper-V Server 2016、Windows Server 2016、Microsoft Hyper-V Server 2019、Windows Server 2019

對 Hyper-v 中的網路功能有基本的瞭解，可協助您規劃虛擬機器的網路功能。 本文也涵蓋使用即時移轉以及搭配其他伺服器功能和角色使用 Hyper-v 時的一些網路考慮。

## <a name="hyper-v-networking-basics"></a>Hyper-v 網路基本概念
Hyper-v 中的基本網路功能相當簡單。 它使用兩個部分：虛擬交換器和虛擬網路介面卡。 您至少需要一個，才能建立虛擬機器的網路功能。 虛擬交換器會連接到任何乙太網路型網路。 虛擬網路介面卡會連接到虛擬交換器上的埠，讓虛擬機器可以使用網路。

建立基本網路的最簡單方式，就是在安裝 Hyper-v 時建立虛擬交換器。 然後，當您建立虛擬機器時，您可以將它連接到交換器。 連接到交換器會自動將虛擬網路介面卡新增到虛擬機器。 如需相關指示，請參閱 [建立 hyper-v 虛擬機器的虛擬交換器](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)。

若要處理不同類型的網路，您可以新增虛擬交換器和虛擬網路介面卡。 所有參數都是 Hyper-v 主機的一部分，但每個虛擬網路介面卡只屬於一部虛擬機器。

虛擬交換器是以軟體為基礎的第2層乙太網路交換器。 它提供內建功能來監視、控制及分割流量，以及安全性和診斷。  您可以藉由安裝外掛程式（也稱為  *延伸*模組）來新增至一組內建功能。 這些都可從獨立軟體廠商取得。 如需有關交換器和延伸模組的詳細資訊，請參閱 [Hyper-v 虛擬交換器](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)。

### <a name="switch-and-network-adapter-choices"></a>交換器和網路介面卡選項
Hyper-v 提供三種類型的虛擬交換器以及兩種類型的虛擬網路介面卡。 您將在建立時選擇您想要的每一個。 您可以使用 Hyper-v 管理員或 Hyper-v 模組進行 Windows PowerShell，以建立及管理虛擬交換器和虛擬網路介面卡。 某些 advanced 網路功能（例如延伸埠存取控制清單 (Acl) ）只能使用 Hyper-v 模組中的 Cmdlet 來管理。

建立虛擬交換器或虛擬網路介面卡之後，您可以對虛擬交換器或虛擬網路介面卡進行一些變更。 例如，您可以將現有的交換器變更為不同的類型，但是這樣做會影響連接到該交換器之所有虛擬機器的網路功能。  因此，除非您犯了錯誤或需要測試某個東西，否則您可能不會這麼做。 另一個範例是，您可以將虛擬網路介面卡連線至不同的交換器，如果您想要連線到不同的網路，您可能會這麼做。 但是，您無法將虛擬網路介面卡從一種類型變更為另一種類型。 您可以新增另一個虛擬網路介面卡，並選擇適當的類型，而不是變更類型。

虛擬交換器類型為：

-   **外部虛擬交換器** -藉由系結至實體網路介面卡，連接至有線實體網路。

-   **內部虛擬交換器** -連接到網路，此網路只能由在具有虛擬交換器之主機上執行的虛擬機器，以及在主機和虛擬機器之間使用。

-   **私人虛擬交換器** -連接到網路，此網路只能由在具有虛擬交換器之主機上執行的虛擬機器使用，但不會提供主機與虛擬機器之間的網路功能。

虛擬網路介面卡類型為：

-   **Hyper-v 特定網路介面卡** -適用于第1代和第2代虛擬機器。 它是專為 Hyper-v 所設計，而且需要 Hyper-v integration services 內含的驅動程式。 這種類型的網路介面卡，是建議的選擇，除非您需要開機至網路或執行不受支援的客體作業系統。 必要的驅動程式僅提供給支援的客體作業系統。 請注意，在 Hyper-v 管理員和網路 Cmdlet 中，這種類型只稱為網路介面卡。

-   **傳統網路介面卡** ：僅適用于第1代虛擬機器。 模擬 Intel 以21140為基礎的 PCI 快速乙太網路介面卡，可用來開機到網路，因此您可以從服務（例如 Windows 部署服務）安裝作業系統。

## <a name="hyper-v-networking-and-related-technologies"></a>Hyper-v 網路和相關技術
最新的 Windows Server 版本引進了改良功能，可提供您更多選項來設定 Hyper-v 的網路功能。 例如，Windows Server 2012 引進了交集網路功能的支援。 這可讓您透過一個外部虛擬交換器路由傳送網路流量。 Windows Server 2016 的建立方式，是在系結至 Hyper-v 虛擬交換器的網路介面卡上，允許遠端直接記憶體存取 (RDMA) 。 您可以使用此設定來搭配或不使用 Switch Embedded 小組 (設定) 。 如需詳細資訊，請參閱 [&#40;RDMA 的遠端直接記憶體存取&#41; 和切換內嵌小組 &#40;設定&#41;](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)

某些功能依賴特定的網路設定，或在某些設定下更好。 規劃或更新您的網路基礎結構時，請考慮這些事項。

**容錯移轉** 叢集-最佳作法是隔離叢集流量，並使用虛擬交換器上的 Hyper-v 服務品質 (QoS) 。 如需詳細資訊，請參閱 Hyper-v 叢集的 [網路建議](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn550728(v=ws.11))

**即時移轉** -使用效能選項來減少網路和 CPU 使用量，以及完成即時移轉所需的時間。 如需相關指示，請參閱 [設定主機以進行即時移轉，而不需要容錯移轉](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)叢集。

**儲存空間直接存取** -此功能依賴 smb 3.0 網路通訊協定和 RDMA。 如需詳細資訊，請參閱 [Windows Server 2016 中的儲存空間直接存取](../../../storage/storage-spaces/storage-spaces-direct-overview.md)。