---
title: Linux 虛擬機器的考量
description: Linux 和 BSD 的虛擬機器
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cc6aab7825754579269eb05e591ca2a3cf5a561b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869679"
---
# <a name="linux-virtual-machine-considerations"></a>Linux 虛擬機器的考量

Linux 和 BSD 虛擬機器有相較於 Windows 虛擬機器在 HYPER-V 中的其他考量。

Integration Services 是否存在，或 VM 是否正在執行模擬的硬體上只使用沒有啟蒙的第一個考量。 Linux 和 BSD 有內建或可下載的 Integration Services 的版本的資料表位於[支援的 Linux 和 FreeBSD 虛擬機器，在 Windows 上的 hyper-v](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)。 這些頁面都有可用的 HYPER-V 功能提供 Linux 發行版本，並附註的這些功能適用的方格。

即使客體執行 Integration Services，它可以與舊版的硬體，這不會呈現最佳的效能設定。 比方說，設定及使用虛擬乙太網路介面卡用於進行來賓，而不是使用傳統網路介面卡。 使用 Windows Server 2016，進階像也會使用 SR-IOV 網路功能。

## <a name="linux-network-performance"></a>Linux 上的網路效能

預設 Linux 啟用硬體加速功能，並依預設會將卸載。 如果內容中的主機上的 NIC 啟用 vRSS，但 Linux 客體能夠使用 vRSS 將會啟用此功能。 在 Powershell 中可以變更這個相同的參數與`EnableNetAdapterRSS`命令。

同樣地，啟用 VMMQ (虛擬交換器 RSS) 功能，在來賓所使用的實體 NIC**屬性** > **設定...**  > **進階**索引標籤 > 設定**虛擬交換器 RSS**至**已啟用**或啟用 VMMQ 在 Powershell 中使用下列：

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

在客體中可以藉由增加限制執行額外的 TCP 微調。 為了達到最佳效能的工作負載散佈到多個 Cpu 和深度的工作負載會產生最佳輸送量，因為虛擬化工作負載會有較高的延遲，比 「 裸機 」 的。

已用於使用網路基準測試某些範例微調參數包括：

```PowerShell
net.core.netdev_max_backlog = 30000
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_wmem = 4096 12582912 33554432
net.ipv4.tcp_rmem = 4096 12582912 33554432
net.ipv4.tcp_max_syn_backlog = 80960
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535
net.ipv4.tcp_abort_on_overflow = 1
```

有用的工具，網路 microbenchmarks 是 ntttcp，這是適用於 Linux 和 Windows。 Linux 版本是開放原始碼，而且可從[ntttcp--linux github.com 上](https://github.com/Microsoft/ntttcp-for-linux)。 Windows 版本可在[下載中心](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)。 微調工作負載時最好是視需要使用多個資料流來獲得最佳的輸送量。 使用模型流量的 ntttcp`-P`參數會設定使用的平行連接數目。

## <a name="linux-storage-performance"></a>Linux 儲存體效能

一些最佳作法，如下所示，就會列在[最佳做法適用於在 HYPER-V 上執行的 Linux](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v)。 Linux 核心會有不同的 I/O 排程器，以重新排列要求不同的演算法。 NOOP 是先進先出佇列傳遞由 hypervisor 所進行的排程決策。 建議您在 HYPER-V 上執行 Linux 虛擬機器時，使用 NOOP 排程器為。 若要變更特定的裝置，開機載入器的組態中的排程器 (/ etc/grub.conf，例如)，新增`elevator=noop`核心參數，並然後重新啟動。

類似於網路，使用儲存體的 Linux 客體效能權益最多個佇列到讓主機保持忙碌。 Microbenchmarking 儲存體效能，您最好使用與 libaio 引擎 fio 效能評定工具。

## <a name="see-also"></a>另請參閱

-   [HYPER-V 術語](terminology.md)

-   [HYPER-V 架構](architecture.md)

-   [HYPER-V 伺服器組態](configuration.md)

-   [HYPER-V 處理器效能](processor-performance.md)

-   [HYPER-V 記憶體效能](memory-performance.md)

-   [HYPER-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [HYPER-V 網路 I/O 效能](network-io-performance.md)

-   [虛擬化環境中偵測瓶頸](detecting-virtualized-environment-bottlenecks.md)
