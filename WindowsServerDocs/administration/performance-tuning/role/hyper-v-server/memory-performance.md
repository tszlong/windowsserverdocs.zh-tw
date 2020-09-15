---
title: Hyper-v 記憶體效能
description: 效能微調 Hyper-v 的記憶體考慮
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f0358747002eb850283c63770885d38872b7b903
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078264"
---
# <a name="hyper-v-memory-performance"></a>Hyper-v 記憶體效能


虛擬程式會將來賓實體記憶體虛擬化，以將虛擬機器彼此隔離，並為每個客體作業系統提供連續的零型記憶體空間，就像在非虛擬化系統上一樣。

## <a name="correct-memory-sizing-for-child-partitions"></a>修正子磁碟分割的記憶體大小調整

您應該將虛擬機器記憶體的大小調整為您通常為實體電腦上的伺服器應用程式所做的調整。 您必須調整其大小以合理處理一般和尖峰時間的預期負載，因為記憶體不足可能會大幅增加回應時間和 CPU 或 i/o 使用量。

您可以啟用動態記憶體，讓 Windows 動態調整虛擬機器記憶體的大小。 使用動態記憶體時，如果虛擬機器中的應用程式在進行海量儲存體配置時遇到問題，您可以增加虛擬機器的頁面檔案大小，以確保在動態記憶體回應記憶體壓力的情況下進行暫時性的備份。

如需動態記憶體的詳細資訊，請參閱 [hyper-v 動態記憶體總覽]( https://go.microsoft.com/fwlink/?linkid=834434) 和 [Hyper-v 動態記憶體設定指南](https://go.microsoft.com/fwlink/?linkid=834435)。

在子磁碟分割中執行 Windows 時，您可以使用子分割內的下列效能計數器，來識別子分割區是否遇到記憶體壓力，而且可能更能以較高的虛擬機器記憶體大小執行。

| 效能計數器                                                         | 建議的臨界值                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 記憶體–待命快取保留字節                                        | 在具有 2 GB （含）以上可見 RAM 的系統上300，待命快取保留字節和可用和零頁面清單位元組的總和應為 200 MB 或以上。 |
| 記憶體–可用 & 零頁面清單位元組                                        | 在具有 2 GB （含）以上可見 RAM 的系統上300，待命快取保留字節和可用和零頁面清單位元組的總和應為 200 MB 或以上。 |
| 記憶體-每秒輸入頁數                                                    | 1小時期間的平均小於10。                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>更正根磁碟分割的記憶體大小調整

根磁碟分割必須有足夠的記憶體，以提供 i/o 虛擬化、虛擬機器快照和管理等服務，以支援子磁碟分割。

Windows Server 2016 中的 hyper-v 會監視根分割區管理作業系統的執行時間健全狀況，以判斷可以安全地配置給子磁碟分割的記憶體數量，同時仍能確保根磁碟分割的高效能和可靠性。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
