---
title: 偵測虛擬化環境中的瓶頸
description: 如何偵測並解決可能的 Hyper-v 效能瓶頸
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 11b2f596fc8c1f8c193100e4a9f1ee792d3d7502
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471413"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>偵測虛擬化環境中的瓶頸

本節應該會提供您一些關於使用效能監視器監視之事項的提示，以及如何找出主機或部分虛擬機器未如預期般執行時問題的可能原因。

## <a name="processor-bottlenecks"></a>處理器瓶頸

以下是一些可能會造成處理器瓶頸的常見案例：

-   一或多個邏輯處理器已載入

-   一或多個虛擬處理器已載入

您可以使用主機的下列效能計數器：

-   邏輯處理器使用率- \\ Hyper-v 虛擬機器邏輯處理器（ \* ） \\ % 總執行時間

-   虛擬處理器使用率- \\ hyper-v 虛擬處理器（ \* ） \\ % 總執行時間

-   根虛擬處理器使用率- \\ hyper-v 虛擬機器根虛擬處理器（ \* ） \\ % 總執行時間

如果**Hyper-v 虛擬機器邏輯處理器（ \_ 總計） \\ % total Runtime**計數器超過90%，則會多載主機。 您應該增加更多處理能力，或將部分虛擬機器移至不同的主機。

如果所有虛擬處理器的**Hyper-v 虛擬處理器（VM 名稱： VP x） \\ % Total Runtime**計數器大於90%，您應該執行下列動作：

-   確認主機未超載

-   找出工作負載是否可以利用更多的虛擬處理器

-   將更多虛擬處理器指派給虛擬機器

若為部分（而非全部）的虛擬處理器， **hyper-v 虛擬處理器（VM 名稱： VP x） \\ % Total Runtime**計數器大於90%，您應該執行下列動作：

-   如果您的工作負載需要大量網路，您應該考慮使用 vRSS。

-   如果虛擬機器未執行 Windows Server 2012 R2，您應該新增更多網路介面卡。

-   如果您的工作負載需要大量儲存空間，您應該啟用虛擬 NUMA 並新增更多虛擬磁片。

如果部分的**hyper-v 虛擬機器根虛擬處理器（根副總 x） \\ % Total Runtime**計數器超過90% 但並非所有虛擬處理器和**處理器（x）% 的插斷 \\ 時間和處理器（x） \\ % DPC Time**計數器大約增加了**根虛擬處理器（根 VP x） \\ % Total Runtime**計數器的值，您應該確定在網路介面卡上啟用 VMQ。

## <a name="memory-bottlenecks"></a>記憶體瓶頸

以下是一些可能會造成記憶體瓶頸的常見案例：

-   主機沒有回應。

-   無法啟動虛擬機器。

-   虛擬機器耗盡記憶體。

您可以使用主機的下列效能計數器：

-   記憶體 \\ 可用 mb

-   Hyper-v 動態記憶體平衡器（ \* ） \\ 可用的記憶體

您可以使用虛擬機器中的下列效能計數器：

-   記憶體 \\ 可用 mb

如果**記憶體 \\ 可用 Mb**和**hyper-v 動態記憶體平衡器（）主機上的 \* \\ 可用記憶體**計數器不足，您應該停止非必要的服務，並將一或多部虛擬機器遷移至另一部主機。

如果虛擬機器中的**記憶體 \\ 可用 mb**計數器很低，您應該指派更多記憶體給虛擬機器。 如果您使用動態記憶體，您應該增加 [記憶體上限] 設定。

## <a name="network-bottlenecks"></a>網路瓶頸

以下是一些可能會造成網路瓶頸的常見案例：

-   主機是網路系結。

-   虛擬機器是網路系結。

您可以使用主機的下列效能計數器：

-   網路介面（*網路介面卡名稱*） \\ 位元組/秒

您可以使用虛擬機器中的下列效能計數器：

-   Hyper-v 虛擬網路介面卡（*虛擬機器名稱 &lt; GUID &gt; *） \\ 位元組/秒

如果 [**實體 NIC 位元組/秒**] 計數器大於或等於90% 的容量，您應該新增額外的網路介面卡、將虛擬機器遷移至另一部主機，以及設定網路 QoS。

如果 [ **hyper-v 虛擬網路介面卡位元組/秒**] 計數器大於或等於 250 MBps，您應該在虛擬機器中新增其他組合的網路介面卡、啟用 vRSS，然後使用 sr-iov。

如果您的工作負載無法滿足其網路延遲，請啟用 SR-IOV 以向虛擬機器出示實體網路介面卡資源。

## <a name="storage-bottlenecks"></a>儲存瓶頸

以下是一些可能會造成儲存瓶頸的常見案例：

-   主機和虛擬機器作業的速度很慢或超時。

-   虛擬機器變慢。

您可以使用主機的下列效能計數器：

-   實體磁片（*磁片磁碟機號*） \\ Avg. Disk sec/Read

-   實體磁片（*磁片磁碟機號*） \\ Avg. Disk sec/Write

-   實體磁片（*磁片磁碟機號*） \\ 平均磁片讀取佇列長度

-   實體磁片（*磁片磁碟機號*） \\ 平均磁片寫入佇列長度

如果延遲持續大於50毫秒，您應該執行下列動作：

-   將虛擬機器分散到額外的儲存體

-   考慮購買更快的儲存空間

-   考慮 Windows Server 2012 R2 中引進的分層式儲存空間

-   請考慮使用 Windows Server 2012 R2 中引進的存放裝置 QoS

-   使用 VHDX

## <a name="additional-references"></a>其他參考

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
