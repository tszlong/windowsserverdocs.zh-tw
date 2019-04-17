---
title: 磁碟機的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589750"
---
# <a name="performance-history-for-drives"></a>磁碟機的效能歷程記錄

> 適用於： Windows Server 內部預覽

[效能歷程記錄的儲存空間直接](performance-history.md)此子主題會說明在收集的磁碟機的效能歷程記錄的詳細資料。 可用的每個磁碟機在叢集中的儲存子系統，不論匯流排效能歷程記錄或媒體類型。 不過，它不適用於 OS 開機磁碟機。

   > [!NOTE]
   > 效能歷程記錄不能收集的磁碟機中已關閉的伺服器。 集合將會繼續自動時伺服器而言 back up。

## <a name="series-names-and-units"></a>數列名稱及單位

這些數列收集每個合格的磁碟機：

| 一系列                          | 單位             |
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
| `physicaldisk.size.total`       |  位元組            |
| `physicaldisk.size.used`        |  位元組            |

## <a name="how-to-interpret"></a>如何轉譯

| 一系列                          | 如何轉譯                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | 完成磁碟每秒讀取作業數目。                |
| `physicaldisk.iops.write`       | 每秒完成的磁碟機的寫入作業數目。               |
| `physicaldisk.iops.total`       | 總數讀取或寫入作業每秒磁碟機來完成。 |
| `physicaldisk.throughput.read`  | 從每秒磁碟讀取的資料量。                            |
| `physicaldisk.throughput.write` | 每秒磁碟寫入資料的數量。                           |
| `physicaldisk.throughput.total` | 總資料讀取或寫入至磁碟每秒的數量。        |
| `physicaldisk.latency.read`     | 從磁碟讀取作業的平均延遲。                          |
| `physicaldisk.latency.write`    | 磁碟機的寫入作業的平均延遲。                           |
| `physicaldisk.latency.average`  | 或磁碟機中的所有作業的平均延遲。                     |
| `physicaldisk.size.total`       | 磁碟機的總計的儲存容量。                                    |
| `physicaldisk.size.used`        | 使用過的儲存容量的磁碟機。                                     |

## <a name="where-they-come-from"></a>他們來自的位置

`iops.*`、 `throughput.*`，及`latency.*`一系列的收集方法從`Physical Disk`效能計數器設定伺服器上的磁碟機連線的位置、 磁碟機每一個執行個體。 這些計數器來計算單位是`partmgr.sys`並沒有包含太多的 Windows 軟體堆疊或任何網路躍點。 他們是裝置硬體效能的人。

| 一系列                          | 來源效能計數器           |
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
   > 計數器計算單位是透過整個間隔，不時。 例如，如果磁碟機的閒置時間 9 秒但完成 30 IOs 第 10 秒，其`physicaldisk.iops.total`將為每秒 3 IOs 此 10 秒間隔期間的平均會記錄。 這可確保其效能歷程記錄擷取所有的活動和很完善至雜訊。

`size.*`一系列的收集方法從`MSFT_PhysicalDisk`WMI，個別的磁碟機的一個執行個體中的類別。

| 一系列                          | Source 屬性        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用狀況

使用[Get PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk)指令程式：

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能的儲存空間直接的歷程記錄](performance-history.md)
