---
title: 虛擬化環境中偵測瓶頸
description: 如何偵測及解決潛在的 hyper-v 效能瓶頸
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867529"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>虛擬化環境中偵測瓶頸

本節會提供一些提示上使用效能監視器來監視及如何找出其中的問題時可能的主機或一些虛擬機器不會執行您會需要。

## <a name="processor-bottlenecks"></a>處理器瓶頸狀況

以下是一些可能會造成處理器瓶頸狀況的常見案例：

-   載入一或多個邏輯處理器

-   載入一或多個虛擬處理器

您可以使用從主機的下列效能計數器：

-   邏輯處理器使用率- \\HYPER-V Hypervisor 邏輯處理器 (\*)\\執行時間總計 %

-   虛擬處理器使用率- \\HYPER-V Hypervisor 的虛擬處理器 (\*)\\執行時間總計 %

-   根虛擬處理器使用率- \\HYPER-V Hypervisor 根虛擬處理器 (\*)\\執行時間總計 %

如果**HYPER-V Hypervisor 邏輯處理器 (\_總計)\\總執行時間 %** 計數器超過 90%，主應用程式多載。 您應該新增更多處理能力，或將部分虛擬機器移至不同的主機。

如果**HYPER-V Hypervisor 虛擬 Processor(VM Name:VP x)\\%總執行時間**計數器超過 90%的所有虛擬處理器，您應該執行下列動作：

-   請確認主機不會多載

-   了解是否工作負載可以利用更多虛擬處理器

-   將更多虛擬處理器指派給虛擬機器

如果**HYPER-V Hypervisor 虛擬 Processor(VM Name:VP x)\\%總執行時間**計數器超過 90%的部分，但不是全部虛擬處理器，您應該執行下列動作：

-   如果您的工作負載會收到網路高用量，您應該考慮使用 vRSS。

-   如果虛擬機器不執行 Windows Server 2012 R2，您應該新增更多的網路介面卡。

-   如果您的工作負載是大量的儲存體，您應該啟用虛擬 NUMA，並新增更多的虛擬磁碟。

如果**HYPER-V Hypervisor 根虛擬處理器 (根副總裁 x)\\%總執行時間**計數器已超過 90%的部分，而不是全部的虛擬處理器和**處理器 (x)\\%Interrupt Time 及處理器 (x)\\%DPC Time**計數器大約加總的值**根虛擬 Processor(Root VP x)\\總執行時間 %** 計數器，您應該確定上啟用 VMQ網路介面卡。

## <a name="memory-bottlenecks"></a>記憶體瓶頸

以下是一些常見案例，可能會導致記憶體瓶頸：

-   主機沒有回應。

-   無法啟動虛擬機器。

-   虛擬機器用完記憶體。

您可以使用從主機的下列效能計數器：

-   記憶體\\Available mbytes

-   HYPER-V 動態記憶體平衡器 (\*)\\可用的記憶體

您可以使用下列效能計數器從虛擬機器：

-   記憶體\\Available mbytes

如果**記憶體\\Available Mbytes**並**HYPER-V 動態記憶體平衡器 (\*)\\可用記憶體**計數器都在主機上很低，您應該停止非必要服務，並將一或多個虛擬機器移轉至另一部主機。

如果**記憶體\\Available Mbytes**計數器很低的虛擬機器中，您應該將更多的記憶體指派給虛擬機器。 如果您使用動態記憶體，您應該增加最大記憶體設定。

## <a name="network-bottlenecks"></a>網路瓶頸

以下是一些常見案例，可能會導致網路效能瓶頸：

-   主應用程式是繫結的網路。

-   虛擬機器的網路繫結。

您可以使用從主機的下列效能計數器：

-   網路介面 (*網路介面卡名稱*)\\Bytes/sec

您可以使用下列效能計數器從虛擬機器：

-   HYPER-V 虛擬網路介面卡 (*虛擬機器名稱&lt;GUID&gt;*)\\Bytes/sec

如果**實體 NIC Bytes/sec**計數器大於或等於容量的 90%，您應該加入額外的網路介面卡、 虛擬機器移轉至另一部主機，及設定網路 QoS。

如果**HYPER-V 虛擬網路介面卡 Bytes/sec**計數器大於或等於 250 MBps，您應該新增其他的小組的網路介面卡，在虛擬機器中，啟用 vRSS，並使用 SR-IOV。

如果您的工作負載無法符合他們的網路延遲，啟用 SR-IOV，呈現給虛擬機器的實體網路介面卡資源。

## <a name="storage-bottlenecks"></a>儲存體瓶頸

以下是一些常見案例，可能會導致儲存體瓶頸：

-   主機和虛擬機器的項目，作業會變慢或逾時。

-   虛擬機器的效能不彰。

您可以使用從主機的下列效能計數器：

-   實體磁碟 (*磁碟代號*)\\avg.disk sec/Read

-   實體磁碟 (*磁碟代號*)\\avg.disk sec/Write

-   實體磁碟 (*磁碟代號*)\\平均磁碟讀取佇列長度

-   實體磁碟 (*磁碟代號*)\\平均磁碟寫入佇列長度

如果持續大於 50 毫秒延遲，您應該執行下列作業：

-   虛擬機器分散到額外的儲存體

-   請考慮購買更快的儲存體

-   請考慮階層式儲存空間，Windows Server 2012 R2 中引進

-   請考慮使用 Windows Server 2012 R2 中引進的儲存體 QoS

-   使用 VHDX

## <a name="see-also"></a>另請參閱

-   [HYPER-V 術語](terminology.md)

-   [HYPER-V 架構](architecture.md)

-   [HYPER-V 伺服器組態](configuration.md)

-   [HYPER-V 處理器效能](processor-performance.md)

-   [HYPER-V 記憶體效能](memory-performance.md)

-   [HYPER-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [HYPER-V 網路 I/O 效能](network-io-performance.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
