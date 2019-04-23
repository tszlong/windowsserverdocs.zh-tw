---
title: 磁碟區的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882949"
---
# <a name="performance-history-for-volumes"></a>磁碟區的效能歷程記錄

> 適用於：Windows Server Insider Preview

子主題[效能歷程記錄的儲存空間直接存取](performance-history.md)詳細說明針對磁碟區所收集的效能歷程記錄。 使用適用於每個叢集共用磁碟區 (CSV) 叢集中的效能歷程記錄。 不過，它不適用於作業系統開機磁碟區或任何其他非 CSV 存放裝置。

   > [!NOTE]
   > 可能需要幾分鐘的時間開始針對新建立或重新命名磁碟區的集合。

## <a name="series-names-and-units"></a>數列名稱與單位

每個符合資格的磁碟區會收集這些系列：

| 系列                    | 單位             |
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

## <a name="how-to-interpret"></a>如何解譯

| 系列                    | 如何解譯                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | 完成此磁碟區每秒的讀取作業數目。                |
| `volume.iops.write`       | 完成此磁碟區每秒的寫入作業數目。               |
| `volume.iops.total`       | 讀取或寫入此磁碟區來完成每秒的作業總數。 |
| `volume.throughput.read`  | 從這個磁碟區每秒讀取的資料數量。                            |
| `volume.throughput.write` | 此磁碟區每秒寫入的資料數量。                           |
| `volume.throughput.total` | 資料讀取或寫入此磁碟區每秒的總數量。        |
| `volume.latency.read`     | 從這個磁碟區的讀取作業的平均延遲。                          |
| `volume.latency.write`    | 此磁碟區的寫入作業的平均延遲。                           |
| `volume.latency.average`  | 所有作業，或從這個磁碟區的平均延遲。                     |
| `volume.size.total`       | 磁碟區總儲存體容量。                                     |
| `volume.size.available`   | 磁碟區可用的儲存體容量。                                 |

## <a name="where-they-come-from"></a>各來源來自

`iops.*`， `throughput.*`，並`latency.*`系列會收集從`Cluster CSVFS`效能計數器集合。 在叢集中的每一部伺服器具有每個 CSV 磁碟區，無論擁有權的執行個體。 效能歷程記錄磁碟區`MyVolume`彙總的`MyVolume`叢集中的每部伺服器上的執行個體。

| 系列                    | 來源計數器         |
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
   > 計數器會測量整個間隔，不取樣。 例如，如果磁碟區的閒置時間 9 秒但完成 30 IOs 10 秒，其`volume.iops.total`將為每秒的 3 個 IOs 10 秒間隔期間的平均會記錄。 這可確保其效能歷程記錄會擷取所有的活動，並是強固的雜訊。

   > [!TIP]
   > 這些是使用熱門的相同計數器[VM Fleet](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1)基準測試架構。

`size.*`序列會收集從`MSFT_Volume`在 WMI 中，每個磁碟區的一個執行個體的類別。

| 系列                    | Source 屬性 |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用[Get-volume](https://docs.microsoft.com/powershell/module/storage/get-volume) cmdlet:

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能歷程記錄的儲存空間直接存取](performance-history.md)
