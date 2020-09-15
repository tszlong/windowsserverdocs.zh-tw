---
title: 偵測虛擬化環境中的瓶頸
description: 如何偵測和解決潛在的 Hyper-v 效能瓶頸
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cc072d51623d53539bcf27dfaf950e4ba1621e71
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077245"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>偵測虛擬化環境中的瓶頸

本節應提供您一些有關使用效能監視器監視的提示，以及如何識別當主機或部分虛擬機器未如預期般執行時，可能發生問題的位置。

## <a name="processor-bottlenecks"></a>處理器瓶頸

以下是一些可能會造成處理器瓶頸的常見案例：

-   已載入一或多個邏輯處理器

-   已載入一或多個虛擬處理器

您可以使用主機的下列效能計數器：

-   邏輯處理器使用率- \\ Hyper-v 虛擬程式邏輯處理器 (\*) \\ % 總執行時間

-   虛擬處理器使用率- \\ Hyper-v 虛擬處理器虛擬處理器 (\*) \\ % 總執行時間

-   根虛擬處理器使用率- \\ hyper-v 虛擬程式根虛擬處理器 (\*) \\ % 總執行時間

如果 **Hyper-v 虛擬程式邏輯處理器 (\_ 總計) \\ % 總運行** 時間計數器超過90%，則會將主機超載。 您應該增加更多處理能力，或將某些虛擬機器移到不同的主機。

如果 **hyper-v 虛擬處理器虛擬處理器 (VM 名稱：所有虛擬處理器的 VP x) \\ % 總運行** 時間計數器超過90%，您應該執行下列作業：

-   確認主機未超載

-   瞭解工作負載是否可利用更多虛擬處理器

-   指派更多虛擬處理器給虛擬機器

如果 **hyper-v 虛擬處理器虛擬處理器 (VM 名稱) ： \\ ** 部分（而非全部）虛擬處理器的總執行時間計數器總數超過90%，您應該執行下列作業：

-   如果您的工作負載需要大量網路，您應該考慮使用 vRSS。

-   如果虛擬機器不是執行 Windows Server 2012 R2，您應該新增更多的網路介面卡。

-   如果您的工作負載需要大量儲存空間，您應該啟用虛擬 NUMA 並新增更多虛擬磁片。

如果 **hyper-v 虛擬程式根虛擬處理器 (根副總 x) \\ % 的總運行** 時間計數器超過90% 的部分，但不是所有虛擬處理器和 **處理器 (x) % 插斷 \\ 時間和處理器 (x) \\ % DPC Time** 計數器大約會新增至 **根虛擬處理器的值 (根 VP x) \\ % Total Runtime** 計數器，您應該確定在網路介面卡上啟用 VMQ。

## <a name="memory-bottlenecks"></a>記憶體瓶頸

以下是一些可能會造成記憶體瓶頸的常見案例：

-   主機沒有回應。

-   無法啟動虛擬機器。

-   虛擬機器的記憶體用盡。

您可以使用主機的下列效能計數器：

-   記憶體 \\ 可用 mb

-   Hyper-v 動態記憶體平衡器 (\*) \\ 可用的記憶體

您可以從虛擬機器使用下列效能計數器：

-   記憶體 \\ 可用 mb

如果 **記憶體 \\ 可用 Mb** 和 **hyper-v 動態記憶體平衡器 () 的 \* \\ 可用記憶體** 計數器不足，您應該停止非必要的服務，並將一或多部虛擬機器遷移至另一部主機。

如果虛擬機器的 **記憶體 \\ 可用 mb** 計數器很低，您應該指派更多的記憶體給虛擬機器。 如果您使用動態記憶體，您應該增加最大記憶體設定。

## <a name="network-bottlenecks"></a>網路瓶頸

以下是一些可能會造成網路瓶頸的常見案例：

-   主機受網路系結。

-   虛擬機器是網路系結的。

您可以使用主機的下列效能計數器：

-   網路介面 (*網路介面卡名稱*) \\ 位元組/秒

您可以從虛擬機器使用下列效能計數器：

-   Hyper-v 虛擬網路介面卡 (*虛擬機器名稱名稱 &lt; GUID &gt; *) \\ 位元組/秒

如果 **實體 NIC Bytes/sec** 計數器大於或等於90% 的容量，您應該新增額外的網路介面卡、將虛擬機器遷移至另一部主機，並設定網路 QoS。

如果 **Hyper-v 虛擬網路介面卡的 Bytes/sec** 計數器大於或等於 250 MBps，您應該在虛擬機器中新增其他組合的網路介面卡、啟用 vRSS，然後使用 sr-iov。

如果您的工作負載不符合其網路延遲，可讓 SR-IOV 將實體網路介面卡資源呈現給虛擬機器。

## <a name="storage-bottlenecks"></a>儲存體瓶頸

以下是一些可能會導致儲存體瓶頸的常見案例：

-   主機與虛擬機器作業的速度很慢或已超時。

-   虛擬機器的速度很慢。

您可以使用主機的下列效能計數器：

-   實體磁片 (*磁片磁碟機*) \\ Avg. Disk sec/Read

-   實體磁片 (*磁片磁碟機*) \\ Avg. Disk sec/Write

-   實體磁片 (*磁片磁碟機* \\ ，) 平均磁片讀取佇列長度

-   實體磁片 (*磁片磁碟機*) \\ 平均磁片寫入佇列長度

如果延遲持續大於50毫秒，您應該執行下列作業：

-   將虛擬機器分散到額外的儲存體

-   考慮購買更快的儲存體

-   考慮在 Windows Server 2012 R2 中引進的分層式儲存空間

-   考慮使用 Windows Server 2012 R2 中引進的儲存體 QoS

-   使用 VHDX

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
