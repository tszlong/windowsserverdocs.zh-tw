---
title: Linux 虛擬機器考慮
description: Linux 和 BSD 虛擬機器
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5668629e7eded214525561d30fec496a4e91b8dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385075"
---
# <a name="linux-virtual-machine-considerations"></a>Linux 虛擬機器考慮

相較于 Hyper-v 中的 Windows 虛擬機器，Linux 和 BSD 虛擬機器有其他考慮。

第一個考慮是 Integration Services 是否存在，或 VM 是否只在沒有啟蒙教學的模擬硬體上執行。 [Windows 上的 Hyper-v 支援的 linux 和 FreeBSD 虛擬機器](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)中有提供內建或可下載 Integration Services 的 LINUX 和 BSD 版本資料表。 這些頁面有適用于 Linux 發行版本的可用 Hyper-v 功能的方格，以及這些功能的相關注意事項。

即使來賓正在執行 Integration Services，它仍可使用舊版硬體進行設定，而不會展現最佳效能。 例如，針對來賓設定和使用虛擬 ethernet 介面卡，而不是使用傳統網路介面卡。 Windows Server 2016 也提供 SR-IOV 這類 advanced 網路功能。

## <a name="linux-network-performance"></a>Linux 網路效能

Linux 預設會啟用硬體加速和卸載。 如果在主機上的 NIC 屬性中啟用了 vRSS，而 Linux 來賓具有使用 vRSS 的功能，將會啟用此功能。 在 Powershell 中，您可以使用 `EnableNetAdapterRSS` 命令來變更相同的參數。

同樣地，您可以在來賓**屬性**所使用的實體 NIC 上啟用 VMMQ （虛擬交換器 rss）**功能 > [設定]**  > [ **Advanced** ] 索引標籤 > 將**虛擬交換器 RSS**設定為 [**啟用**]，或使用下列命令在 Powershell 中啟用 VMMQ：

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

在中，您可以藉由增加限制來執行來賓額外的 TCP 微調。 為了達到最佳效能，將工作負載分散到多個 Cpu，並具有深度工作負載會產生最佳的輸送量，因為虛擬化工作負載的延遲會高於「裸機」。

一些在網路效能評定中有用的微調參數範例包括：

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

適用于網路 microbenchmarks 的公用程式是 ntttcp，其適用于 Linux 和 Windows。 Linux 版本是開放原始碼，可從[github.com 上的 ntttcp for linux 取得](https://github.com/Microsoft/ntttcp-for-linux)。 您可以在[下載中心](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)找到 Windows 版本。 微調工作負載時，最好盡可能使用所需數量的資料流程來取得最佳的輸送量。 使用 ntttcp 來建立流量模型時，`-P` 參數會設定所使用的平行連接數目。

## <a name="linux-storage-performance"></a>Linux 儲存體效能

在[hyper-v 上執行 Linux 的最佳作法](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v)中列出一些最佳做法，如下所示。 Linux 核心具有不同的 i/o 排程器，可使用不同的演算法重新排序要求。 NOOP 是先進先出佇列，可通過由管理者所進行的排程決策。 建議在 Hyper-v 上執行 Linux 虛擬機器時，使用 NOOP 做為排程器。 若要變更特定裝置的排程器，請在開機載入器的設定（例如/etc/grub.conf）中，將 `elevator=noop` 新增至核心參數，然後重新開機。

與網路類似，Linux 的來賓效能與存放裝置的優點是多個佇列的深度，足以讓主機保持忙碌。 使用 libaio 引擎的 fio 基準測試，Microbenchmarking 儲存體效能可能最佳。

## <a name="see-also"></a>請參閱

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)
