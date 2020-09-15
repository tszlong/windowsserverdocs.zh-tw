---
title: Hyper-v 網路 i/o 效能
description: Hyper-v 效能調整中的網路 i/o 效能考慮
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 10a2c21dc581864796ef2a301033377da09a5f7f
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078245"
---
# <a name="hyper-v-network-io-performance"></a>Hyper-v 網路 i/o 效能

Server 2016 包含數個改善和新功能，可將 Hyper-v 的網路效能優化。  本文的未來版本將包含如何將網路效能優化的相關檔。

## <a name="live-migration"></a>即時移轉

即時移轉可讓您將執行中的虛擬機器從容錯移轉叢集的一個節點透明地移至相同叢集中的另一個節點，而不會中斷網路連線或察覺的停機時間。

> [!NOTE]
> 容錯移轉叢集需要叢集節點的共用存放裝置。

移動執行中虛擬機器的程式可以分成兩個主要階段。 第一個階段會將虛擬機器的記憶體從目前的主機複製到新的主機。 第二個階段會將虛擬機器狀態從目前的主機轉移至新的主機。 這兩個階段的持續時間取決於從目前的主機傳送資料到新主機的速度。

提供專用的網路來進行即時移轉流量有助於將完成即時移轉所需的時間降到最低，並確保一致的遷移時間。

![hyper-v 即時移轉設定範例](../../media/perftune-guide-live-migration.png)

此外，增加遷移時所牽涉到的每個網路介面卡上的傳送和接收緩衝區數目，可改善遷移效能。

Windows Server 2012 R2 引進了一個選項，可在透過網路傳輸之前壓縮記憶體來加速即時移轉，或在您的硬體支援時，將遠端直接記憶體存取 (RDMA) 。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
