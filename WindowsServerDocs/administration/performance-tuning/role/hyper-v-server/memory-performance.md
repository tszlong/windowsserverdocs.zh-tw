---
title: HYPER-V 記憶體效能
description: 在進行效能微調之 HYPER-V 的記憶體考量
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 63a1b654b8ac52725cc5dd87c8b245f9dfaf40f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848069"
---
# <a name="hyper-v-memory-performance"></a>HYPER-V 記憶體效能


Hypervisor 虛擬化來賓的實體記憶體隔離彼此的虛擬機器並將每個客體作業系統，提供連續的以零為起始的記憶體空間，只是因為在非虛擬化系統。

## <a name="correct-memory-sizing-for-child-partitions"></a>子磁碟分割的正確的記憶體大小

依照您平常在實體電腦上的伺服器應用程式，您應該調整大小的虛擬機器的記憶體。 您必須調整大小以合理地處理的預期的負載，一般和尖峰時間，因為記憶體不足可能會大幅增加回應時間和 CPU 或 I/O 使用量。

您可以啟用動態記憶體，以允許 Windows 動態調整虛擬機器記憶體的大小。 使用動態記憶體，如果虛擬機器中的應用程式時遇到問題進行大量的突然記憶體配置，您可以增加分頁檔大小，以確保暫存備份，在回應記憶體壓力的動態記憶體時虛擬機器。

如需動態記憶體的詳細資訊，請參閱[HYPER-V 動態記憶體概觀]( https://go.microsoft.com/fwlink/?linkid=834434)並[HYPER-V 動態記憶體設定指南](https://go.microsoft.com/fwlink/?linkid=834435)。

當執行 Windows 的子磁碟分割中，您可以使用子磁碟分割內的下列效能計數器來識別是否子磁碟分割發生記憶體不足的壓力，且可能會更高的虛擬機器的記憶體大小與效能較佳。

| 效能計數器                                                         | 建議的臨界值                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 記憶體中-待命的快取保留位元組                                        | 200 MB 或以上 1 GB，以及 300 MB 或更多有 2 GB 或更多的可見的 RAM 的系統上的系統上，應該是待命快取保留位元組和免費和零分頁清單位元組的總和。 |
| 記憶體-可用和零分頁清單位元組                                        | 200 MB 或以上 1 GB，以及 300 MB 或更多有 2 GB 或更多的可見的 RAM 的系統上的系統上，應該是待命快取保留位元組和免費和零分頁清單位元組的總和。 |
| 記憶體-Pages Input/Sec                                                    | 在 1 小時內的平均為小於 10。                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>根磁碟分割的正確的記憶體大小

在根磁碟分割必須有足夠的記憶體來提供服務，例如 I/O 虛擬化、 虛擬機器快照和管理以支援子磁碟分割。

Windows Server 2016 中的 HYPER-V 會監視執行階段健全狀況的根磁碟分割的管理作業系統決定多少記憶體可安全地將配置給子磁碟分割，同時仍然確保高的效能和可靠性的根磁碟分割。

## <a name="see-also"></a>另請參閱

-   [HYPER-V 術語](terminology.md)

-   [HYPER-V 架構](architecture.md)

-   [HYPER-V 伺服器組態](configuration.md)

-   [HYPER-V 處理器效能](processor-performance.md)

-   [HYPER-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [HYPER-V 網路 I/O 效能](network-io-performance.md)

-   [虛擬化環境中偵測瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
