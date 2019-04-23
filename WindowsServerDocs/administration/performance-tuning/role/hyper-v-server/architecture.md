---
title: Hyper-V 架構
description: Hyper-v 架構 condsiderations 進行效能微調
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fcc87b04698a44e115c8f49150fe33443f8e6a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890279"
---
# <a name="hyper-v-architecture"></a>Hyper-V 架構

HYPER-V 功能與 Type 1 hypervisor 為基礎的架構。 Hypervisor 虛擬化的處理器和記憶體，並提供根磁碟分割管理子磁碟分割 （虛擬機器），並公開 （expose） 服務，例如 I/O 裝置的虛擬機器的虛擬化堆疊的機制。

在根磁碟分割擁有，且可直接存取實體的 I/O 裝置。 在根磁碟分割的虛擬化堆疊提供虛擬機器、 管理 Api，以及虛擬化的 I/O 裝置的記憶體管理員。 它也實作模擬的裝置，例如整合的裝置電子 (IDE) 磁碟控制卡和 PS/2 輸入的裝置 連接埠，而且它支援 Hyper-v 特定綜合裝置來提升效能並降低額外負荷。

![hyper-v hypervisor 為基礎的架構](../../media/perftune-guide-hyperv-arch.png)

Hyper-V 特定 I/O 架構是由虛擬化服務提供者 (Vsp) 根分割區和虛擬化服務用戶端 (Vsc) 中的子磁碟分割所組成。 每個服務會公開為裝置，透過 VMBus，因此可做為 I/O 匯流排，並啟用使用機制，例如共用記憶體的虛擬機器之間的高效能通訊。 客體作業系統的隨插即用 manager 列舉包括 VMBus，這些裝置，並載入適當的裝置驅動程式 （虛擬服務用戶端）。 透過這種架構，也會公開 I/O 以外的服務。

從 Windows Server 2008，作業系統功能 enlightenment，以最佳化其行為，在虛擬機器中執行時開始。 優點包括減少記憶體虛擬化，改善多核心的延展性，並降低背景的客體作業系統的 CPU 使用量的成本。

下列各節會建議會產生在執行 HYPER-V 角色的伺服器上增加的效能的最佳作法。

## <a name="see-also"></a>另請參閱

-   [HYPER-V 術語](terminology.md)

-   [HYPER-V 伺服器組態](configuration.md)

-   [HYPER-V 處理器效能](processor-performance.md)

-   [HYPER-V 記憶體效能](memory-performance.md)

-   [HYPER-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [HYPER-V 網路 I/O 效能](network-io-performance.md)

-   [虛擬化環境中偵測瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
