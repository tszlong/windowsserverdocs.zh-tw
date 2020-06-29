---
title: Hyper-v 記憶體效能
description: 效能微調 Hyper-v 中的記憶體考慮
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: b513dd3346d593ec4c823808f540bce68dd472ce
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471363"
---
# <a name="hyper-v-memory-performance"></a>Hyper-v 記憶體效能


虛擬程式會將來賓實體記憶體虛擬化，以將虛擬機器彼此隔離，並為每個客體作業系統提供連續、以零為基礎的記憶體空間，就像在非虛擬化系統上一樣。

## <a name="correct-memory-sizing-for-child-partitions"></a>更正子磁碟分割的記憶體大小

您應該調整虛擬機器記憶體的大小，就像在實體電腦上執行伺服器應用程式一般。 您必須調整它的大小，以合理地處理一般和尖峰時間的預期負載，因為記憶體不足可能會大幅增加回應時間和 CPU 或 i/o 使用量。

您可以啟用動態記憶體，讓 Windows 能夠動態調整虛擬機器記憶體的大小。 使用動態記憶體，如果虛擬機器中的應用程式發生大量突然記憶體配置問題，您可以增加虛擬機器的頁面檔案大小，以確保暫時支援，同時動態記憶體回應記憶體壓力。

如需動態記憶體的詳細資訊，請參閱[hyper-v 動態記憶體總覽]( https://go.microsoft.com/fwlink/?linkid=834434)和[Hyper-v 動態記憶體設定指南](https://go.microsoft.com/fwlink/?linkid=834435)。

在子分割區中執行 Windows 時，您可以使用子資料分割內的下列效能計數器，來識別子分割區是否發生記憶體壓力，而且可能會因為較高的虛擬機器記憶體大小而執行得更佳。

| 效能計數器                                                         | 建議的臨界值                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 記憶體–待命快取保留字節                                        | [待命快取保留字節] 和 [可用] 和 [零分頁清單] 的總和，在具有 1 GB 的系統上應為 200 MB 或以上，而在具有 2 GB 或更多可用 RAM 的系統上，則為 300 MB 以上。 |
| 記憶體-免費 & 零頁清單位元組                                        | [待命快取保留字節] 和 [可用] 和 [零分頁清單] 的總和，在具有 1 GB 的系統上應為 200 MB 或以上，而在具有 2 GB 或更多可用 RAM 的系統上，則為 300 MB 以上。 |
| 記憶體–頁數輸入/秒                                                    | 1小時期間的平均值小於10。                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>更正根磁碟分割的記憶體大小

根磁碟分割必須有足夠的記憶體來提供服務，例如 i/o 虛擬化、虛擬機器快照和管理，以支援子磁碟分割。

Windows Server 2016 中的 hyper-v 會監視根磁碟分割管理作業系統的執行時間健全狀況，以決定可以安全地配置給子磁碟分割的記憶體數量，同時仍能確保根磁碟分割的高效能和可靠性。

## <a name="additional-references"></a>其他參考

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
