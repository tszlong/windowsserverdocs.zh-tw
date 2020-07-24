---
title: 磁片磁碟機的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f54f06462818ca21ae10acee40d788211b38e37
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964520"
---
# <a name="performance-history-for-drives"></a>磁片磁碟機的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取的效能歷程記錄](performance-history.md)的子主題會詳細說明針對磁片磁碟機收集的效能歷程記錄。 不論是哪種匯流排或媒體類型，叢集儲存子系統中的每個磁片磁碟機都可以使用效能歷程記錄。 不過，它不適用於 OS 開機磁碟機。

   > [!NOTE]
   > 無法在伺服器關閉的情況下收集磁片磁碟機的效能歷程記錄。 當伺服器恢復運作時，集合會自動繼續。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會針對每個符合資格的磁片磁碟機收集這些系列：

| 數列                          | 單位             |
|---------------------------------|------------------|
| `physicaldisk.iops.read`        | 每秒       |
| `physicaldisk.iops.write`       | 每秒       |
| `physicaldisk.iops.total`       | 每秒       |
| `physicaldisk.throughput.read`  | 每秒位元組數 |
| `physicaldisk.throughput.write` | 每秒位元組數 |
| `physicaldisk.throughput.total` | 每秒位元組數 |
| `physicaldisk.latency.read`     | seconds          |
| `physicaldisk.latency.write`    | seconds          |
| `physicaldisk.latency.average`  | seconds          |
| `physicaldisk.size.total`       | 位元組            |
| `physicaldisk.size.used`        | 位元組            |

## <a name="how-to-interpret"></a>如何解讀

| 數列                          | 如何解讀                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | 磁片磁碟機每秒完成的讀取作業數目。                |
| `physicaldisk.iops.write`       | 磁片磁碟機每秒完成的寫入作業數目。               |
| `physicaldisk.iops.total`       | 磁片磁碟機每秒完成的讀取或寫入作業總數。 |
| `physicaldisk.throughput.read`  | 每秒從磁片磁碟機讀取的資料數量。                            |
| `physicaldisk.throughput.write` | 每秒寫入磁片磁碟機的資料數量。                           |
| `physicaldisk.throughput.total` | 每秒從磁片磁碟機讀取或寫入的總數據量。        |
| `physicaldisk.latency.read`     | 從磁片磁碟機讀取作業的平均延遲。                          |
| `physicaldisk.latency.write`    | 磁片磁碟機寫入作業的平均延遲。                           |
| `physicaldisk.latency.average`  | 磁片磁碟機的所有作業的平均延遲。                     |
| `physicaldisk.size.total`       | 磁片磁碟機的總存放裝置容量。                                    |
| `physicaldisk.size.used`        | 磁片磁碟機的已使用儲存容量。                                     |

## <a name="where-they-come-from"></a>來自何處

`iops.*`、 `throughput.*` 和 `latency.*` 數列會從 `Physical Disk` 連接磁片磁碟機的伺服器上的效能計數器集合中收集，每個磁片磁碟機一個實例。 這些計數器是由測量 `partmgr.sys` ，而且不包含大部分的 Windows 軟體堆疊，也不包括任何網路躍點。 它們代表裝置硬體效能。

| 數列                          | 來源計數器           |
|---------------------------------|--------------------------|
| `physicaldisk.iops.read`        | `Disk Reads/sec`         |
| `physicaldisk.iops.write`       | `Disk Writes/sec`        |
| `physicaldisk.iops.total`       | `Disk Transfers/sec`     |
| `physicaldisk.throughput.read`  | `Disk Read Bytes/sec`    |
| `physicaldisk.throughput.write` | `Disk Write Bytes/sec`   |
| `physicaldisk.throughput.total` | `Disk Bytes/sec`         |
| `physicaldisk.latency.read`     | `Avg. Disk sec/Read`     |
| `physicaldisk.latency.write`    | `Avg. Disk sec/Writes`   |
| `physicaldisk.latency.average`  | `Avg. Disk sec/Transfer` |

   > [!NOTE]
   > 計數器是以整個間隔來測量，而不是取樣。 例如，如果磁片磁碟機在9秒內閒置，但在第10秒內完成30個 Io，則在 `physicaldisk.iops.total` 此10秒的間隔期間，平均會將其記錄為每秒3個 io。 這可確保其效能歷程會捕捉所有活動，而且健全于雜訊。

`size.*`系統會從 `MSFT_PhysicalDisk` WMI 中的類別收集數列，每個磁片磁碟機一個實例。

| 數列                          | Source 屬性        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[PhysicalDisk](/powershell/module/storage/get-physicaldisk) Cmdlet：

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
