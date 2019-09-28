---
title: Hyper-v 網路 i/o 效能
description: Hyper-v 效能調整中的網路 i/o 效能考慮
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e8f4261c11a63786c2d170105fb0fa65dc6966a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385116"
---
# <a name="hyper-v-network-io-performance"></a>Hyper-v 網路 i/o 效能

伺服器2016包含數項改良功能和新功能，可將 Hyper-v 下的網路效能優化。  本文章的未來版本將包含如何將網路效能優化的檔。

## <a name="live-migration"></a>即時移轉

即時移轉可讓您以透明的方式，將執行中的虛擬機器從容錯移轉叢集的一個節點移至相同叢集中的另一個節點，而不會中斷網路連線或察覺到停機時間。

> [!NOTE]
> 容錯移轉叢集需要叢集節點的共用存放裝置。

移動執行中虛擬機器的程式可分為兩個主要階段。 第一個階段會將虛擬機器的記憶體從目前的主機複製到新的主機。 第二個階段會將虛擬機器狀態從目前的主機傳輸至新主機。 這兩個階段的持續時間，都是由從目前的主機傳送資料到新主機的速度而大幅決定。

為即時移轉流量提供私人網路絡有助於將完成即時移轉所需的時間降到最低，並確保一致的遷移時間。

![hyper-v 即時移轉設定範例](../../media/perftune-guide-live-migration.png)

此外，增加與遷移相關的每個網路介面卡上的傳送和接收緩衝區數目，可以改善遷移效能。

Windows Server 2012 R2 引進了一個選項，可在透過網路傳送或使用遠端直接記憶體存取（RDMA）（如果您的硬體支援）時，藉由壓縮記憶體來加速即時移轉。

## <a name="see-also"></a>另請參閱

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
