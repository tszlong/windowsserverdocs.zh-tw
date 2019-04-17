---
title: 磁碟區均為效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589699"
---
# <a name="performance-history-for-volumes"></a>磁碟區均為效能歷程記錄

> 適用於： Windows Server 內部預覽

[效能歷程記錄的儲存空間直接](performance-history.md)此子主題會說明在磁碟區均為收集的效能歷程記錄的詳細資料。 使用的每個叢集共用大量 (CSV) 叢集中的效能歷程記錄。 不過，它不適用於 OS 開機磁碟區或任何其他非 CSV 儲存。

   > [!NOTE]
   > 可能需要數分鐘的開始或重新命名新建立的磁碟區的集合。

## <a name="series-names-and-units"></a>數列名稱及單位

這些數列會針對每個合格的磁碟區收集：

| 一系列                    | 單位             |
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
| `volume.size.total`       |  位元組            |
| `volume.size.available`   |  位元組            |

## <a name="how-to-interpret"></a>如何轉譯

| 一系列                    | 如何轉譯                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | 每秒完成此大量讀取作業數目。                |
| `volume.iops.write`       | 每秒完成此大量寫入作業數目。               |
| `volume.iops.total`       | 總數讀取或寫入作業完成這個磁碟區每秒。 |
| `volume.throughput.read`  | 從這個磁碟區每秒讀取的資料量。                            |
| `volume.throughput.write` | 資料寫入這個磁碟區每秒的數量。                           |
| `volume.throughput.total` | 總資料讀取或寫入這個磁碟區每秒的數量。        |
| `volume.latency.read`     | 從這個大量讀取作業的平均延遲。                          |
| `volume.latency.write`    | 以這個大量寫入作業的平均延遲。                           |
| `volume.latency.average`  | 至或來自此磁碟區的所有作業的平均延遲。                     |
| `volume.size.total`       | 磁碟區總計的儲存容量。                                     |
| `volume.size.available`   | 可用的儲存容量的磁碟區。                                 |

## <a name="where-they-come-from"></a>他們來自的位置

`iops.*`、 `throughput.*`，及`latency.*`一系列的收集方法從`Cluster CSVFS`效能計數器設定。 叢集中的每個伺服器有每個 CSV 數量，無論擁有權的執行個體。 效能歷程記錄的磁碟區`MyVolume`的彙總`MyVolume`叢集中的每台伺服器上的執行個體。

| 一系列                    | 來源效能計數器         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *在上述的總和*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *在上述的總和*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *在上述平均* |

   > [!NOTE]
   > 計數器計算單位是透過整個間隔，不時。 例如，如果磁碟區處於閒置狀態為 9 秒但完成 30 IOs 第 10 秒，其`volume.iops.total`將為每秒 3 IOs 此 10 秒間隔期間的平均會記錄。 這可確保其效能歷程記錄擷取所有的活動和很完善至雜訊。

   > [!TIP]
   > 這些是常用的[VM 艦隊](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1)基準 framework 所使用的相同計數器。

`size.*`一系列的收集方法從`MSFT_Volume`WMI，每個磁碟區的一個執行個體中的類別。

| 一系列                    | Source 屬性 |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用狀況

使用[取得大量](https://docs.microsoft.com/powershell/module/storage/get-volume)指令程式：

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能的儲存空間直接的歷程記錄](performance-history.md)
