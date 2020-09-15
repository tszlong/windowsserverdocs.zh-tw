---
title: Hyper-V 架構
description: 效能微調的 hyper-v 架構 condsiderations
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c51577400b449864f45679423718387598348a71
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077265"
---
# <a name="hyper-v-architecture"></a>Hyper-V 架構

Hyper-v 的功能是一種以類型1的基礎結構為基礎的架構。 虛擬程式會將處理器和記憶體虛擬化，並在根磁碟分割中提供虛擬化堆疊的機制，以管理 (虛擬機器) 的子磁碟分割，並將服務（例如 i/o 裝置）公開給虛擬機器。

根磁碟分割擁有並可直接存取實體 i/o 裝置。 根分割區中的虛擬化堆疊會提供虛擬機器、管理 Api 和虛擬化 i/o 裝置的記憶體管理員。 它也會實作為整合裝置電子 (IDE) 磁碟控制卡和 PS/2 輸入裝置埠的模擬裝置，並支援 Hyper-v 專屬的綜合裝置，以提升效能並降低額外負荷。

![以 hyper-v 基礎架構為基礎的架構](../../media/perftune-guide-hyperv-arch.png)

Hyper-v 特定 i/o 架構是由虛擬化服務提供者所組成 (.Vsps) 在根磁碟分割中，而虛擬化服務用戶端 (Vsc) 子分割區中。 每個服務都會以裝置的形式公開給 VMBus，作為 i/o 匯流排，並在使用共用記憶體等機制的虛擬機器之間啟用高效能通訊。 客體作業系統的隨插即用 manager 會列舉這些裝置（包括 VMBus），並)  (虛擬服務用戶端載入適當的設備磁碟機。 I/o 以外的服務也會透過此架構公開。

從 Windows Server 2008 開始，作業系統功能 enlightenments 在虛擬機器中執行時，將其行為優化。 優點包括降低記憶體虛擬化的成本、改善多核心的擴充性，以及降低客體作業系統的背景 CPU 使用量。

下列各節建議在執行 Hyper-v 角色的伺服器上產生更高效能的最佳做法。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
