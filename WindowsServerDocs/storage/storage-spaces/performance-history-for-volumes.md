---
title: 磁片區的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b12c4e2c23601d6948d4bd2c432cb2c63c605ae
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955160"
---
# <a name="performance-history-for-volumes"></a>磁片區的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取效能歷程記錄](performance-history.md)的子主題詳細說明為磁片區收集的效能歷程記錄。 效能歷程記錄適用于叢集中的每個叢集共用磁碟區（CSV）。 不過，它不適用於 OS 開機磁碟區或其他任何非 CSV 存放裝置。

   > [!NOTE]
   > 可能需要幾分鐘的時間，才能開始收集新建立或重新命名的磁片區。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會針對每個符合資格的磁片區收集這些系列：

| 數列                    | 單位             |
|---------------------------|------------------|
| `volume.iops.read`        | 每秒       |
| `volume.iops.write`       | 每秒       |
| `volume.iops.total`       | 每秒       |
| `volume.throughput.read`  | 每秒位元組數 |
| `volume.throughput.write` | 每秒位元組數 |
| `volume.throughput.total` | 每秒位元組數 |
| `volume.latency.read`     | seconds          |
| `volume.latency.write`    | seconds          |
| `volume.latency.average`  | seconds          |
| `volume.size.total`       | 位元組            |
| `volume.size.available`   | 位元組            |

## <a name="how-to-interpret"></a>如何解讀

| 數列                    | 如何解讀                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | 此磁片區每秒完成的讀取作業數目。                |
| `volume.iops.write`       | 此磁片區每秒完成的寫入作業數目。               |
| `volume.iops.total`       | 此磁片區每秒完成的讀取或寫入作業總數。 |
| `volume.throughput.read`  | 每秒從這個磁片區讀取的資料數量。                            |
| `volume.throughput.write` | 每秒寫入此磁片區的資料數量。                           |
| `volume.throughput.total` | 每秒從這個磁片區讀取或寫入的資料總量。        |
| `volume.latency.read`     | 從此磁片區讀取作業的平均延遲。                          |
| `volume.latency.write`    | 此磁片區寫入作業的平均延遲。                           |
| `volume.latency.average`  | 此磁片區之間所有作業的平均延遲。                     |
| `volume.size.total`       | 磁片區的總儲存容量。                                     |
| `volume.size.available`   | 磁片區的可用儲存容量。                                 |

## <a name="where-they-come-from"></a>來自何處

`iops.*`、 `throughput.*` 和 `latency.*` 數列是從 `Cluster CSVFS` 效能計數器集合收集而來。 叢集中的每一部伺服器都有每個 CSV 磁片區的實例，不論擁有權為何。 針對磁片區記錄的效能歷程記錄，是叢集中每部 `MyVolume` `MyVolume` 伺服器實例的匯總。

| 數列                    | 來源計數器         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *上述的總和*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *上述的總和*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *上述的平均值* |

   > [!NOTE]
   > 計數器是以整個間隔來測量，而不是取樣。 例如，如果磁片區閒置了9秒，但在第10秒內完成30個 Io，則在 `volume.iops.total` 此10秒的間隔期間，平均會將其記錄為每秒3個 io。 這可確保其效能歷程會捕捉所有活動，而且健全于雜訊。

   > [!TIP]
   > 這些是常用[VM 車隊](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1)基準測試架構所使用的相同計數器。

`size.*`系統會從 `MSFT_Volume` WMI 中的類別收集數列，每個磁片區一個實例。

| 數列                    | Source 屬性 |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[取得磁片](/powershell/module/storage/get-volume)區 Cmdlet：

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
