---
title: HYPER-V 網路 I/O 效能
description: 網路 i/o 效能考量，在 HYPER-V 效能微調
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 6dd9d03cce71c42e2ecc1388519de049a5e4e8dd
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266718"
---
# <a name="hyper-v-network-io-performance"></a>HYPER-V 網路 I/O 效能

Server 2016 包含一些增強功能和最佳化網路效能，在 HYPER-V 的新功能。  在這篇文章的未來版本中，將會包含文件說明如何最佳化網路效能。

## <a name="live-migration"></a>即時移轉

即時移轉可讓您以透明的方式從某個節點容錯移轉叢集的執行中虛擬機器移至另一個節點，而不會中斷的網路連線或感受到的停機的相同叢集中。

> [!Note]
> 容錯移轉叢集需要共用存放裝置叢集節點。

移動執行中虛擬機器的程序可以分成兩個主要階段。 第一個階段會複製到新的主機目前的主機中的虛擬機器的記憶體。 第二個階段從目前的主機至新主機轉移虛擬機器的狀態。 這兩個階段的持續時間大幅取決於資料可以傳出至新主機目前的主應用程式的速度。

提供專用的網路進行即時移轉流量降至最低的時間，才可完成即時移轉，並且確保一致的移轉時間。

![hyper-v 即時移轉設定範例](../../media/perftune-guide-live-migration.png)

此外，增加傳送和接收緩衝區上每個網路介面卡在移轉中所包含可以改善移轉效能。

Windows Server 2012 R2 已導入選項壓縮的記憶體，然後再透過網路傳輸來加速即時移轉，或使用遠端直接記憶體存取 (RDMA)，如果您的硬體支援。

## <a name="see-also"></a>另請參閱

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
