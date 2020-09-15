---
title: Linux 虛擬機器考慮
description: Linux 和 BSD 虛擬機器
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: be9d2f0d758f6f59282c770ef1a48d71a947ba00
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078285"
---
# <a name="linux-virtual-machine-considerations"></a>Linux 虛擬機器考慮

相較于 Hyper-v 中的 Windows 虛擬機器，Linux 和 BSD 虛擬機器有其他考慮。

第一個考慮是 Integration Services 是否存在，或 VM 是否只在沒有啟蒙的模擬硬體上執行。 具有內建或可下載 Integration Services 的 Linux 和 BSD 版本資料表可在 [Windows 上的 Hyper-v 支援的 Linux 和 FreeBSD 虛擬機器中取得](../../../../virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)。 這些頁面有適用于 Linux 發行版本的可用 Hyper-v 功能，以及這些功能適用的注意事項。

即使來賓正在執行 Integration Services，也可以使用舊版硬體進行設定，而不會呈現最佳效能。 例如，針對來賓設定和使用虛擬乙太網路介面卡，而不是使用傳統網路介面卡。 使用 Windows Server 2016，您也可以使用 SR-IOV 等 advanced 網路功能。

## <a name="linux-network-performance"></a>Linux 網路效能

Linux 預設會啟用硬體加速和卸載。 如果在主機上的 NIC 的屬性中啟用了 vRSS，而 Linux 來賓具有使用 vRSS 的功能，則會啟用此功能。 在 Powershell 中，您可以使用命令來變更相同的參數 `EnableNetAdapterRSS` 。

同樣地，VMMQ (虛擬交換器 RSS) 功能可在 guest**屬性**所使用的實體 NIC 上啟用 [  >  **設定 ...**  >  ][**設定] 索引**標籤 > 使用下列命令，在 Powershell 中將**虛擬交換器 RSS**設定為**啟用**或啟用 VMMQ：

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

在來賓中，可以藉由增加限制來執行額外的 TCP 調整。 由於虛擬化工作負載的延遲高於「裸機」，因此可讓您在多個 Cpu 上進行最佳的效能分配工作負載，並讓深度工作負載產生最佳的輸送量。

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

適用于網路 microbenchmarks 的公用程式是 ntttcp，可在 Linux 和 Windows 上使用。 Linux 版本是開放原始碼，可從 [ntttcp-linux on github.com 取得](https://github.com/Microsoft/ntttcp-for-linux)。 您可以在 [下載中心](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769)找到 Windows 版本。 微調工作負載時，最好盡可能使用最多的資料流程來取得最佳的輸送量。 使用 ntttcp 來建立流量模型， `-P` 參數會設定所使用的平行連接數目。

## <a name="linux-storage-performance"></a>Linux 儲存體效能

以下是一些最佳作法，如下列範例所示，是在 [hyper-v 上執行 Linux 的最佳作法](../../../../virtualization/hyper-v/best-practices-for-running-linux-on-hyper-v.md)。 Linux 核心具有不同的 i/o 排程器，可使用不同的演算法來重新排序要求。 NOOP 是先進先出的佇列，可將排程決策傳遞給程式管理人員進行。 建議您在 Hyper-v 上執行 Linux 虛擬機器時，使用 NOOP 作為排程器。 若要變更特定裝置的排程器，請在開機載入器的設定 (/etc/grub.conf （例如) ）中新增 `elevator=noop` 至核心參數，然後重新開機。

與網路功能類似，使用儲存體的 Linux 來賓效能可充分利用有足夠深度的多個佇列，讓主機保持忙碌。 使用 libaio 引擎的 fio 基準測試，Microbenchmarking 儲存體效能可能最適合。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)