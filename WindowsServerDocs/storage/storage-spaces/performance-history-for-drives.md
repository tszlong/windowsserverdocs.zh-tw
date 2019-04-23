---
title: 磁碟機的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879189"
---
# <a name="performance-history-for-drives"></a>磁碟機的效能歷程記錄

> 適用於：Windows Server Insider Preview

子主題[效能歷程記錄的儲存空間直接存取](performance-history.md)詳細說明收集磁碟機的效能歷程記錄。 效能歷程記錄可供叢集儲存體子系統，不論匯流排中的每個磁碟機或媒體類型。 不過，它不適用於作業系統開機磁碟機。

   > [!NOTE]
   > 無法收集效能歷程記錄已關閉的伺服器中的磁碟機。 集合將會自動繼續時伺服器恢復運作。

## <a name="series-names-and-units"></a>數列名稱與單位

這些數列會收集每個符合資格的磁碟機：

| 系列                          | 單位             |
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

## <a name="how-to-interpret"></a>如何解譯

| 系列                          | 如何解譯                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | 每秒完成的磁碟機的讀取作業數目。                |
| `physicaldisk.iops.write`       | 每秒完成的磁碟機的寫入作業數目。               |
| `physicaldisk.iops.total`       | 讀取或寫入作業每秒完成的磁碟機的總數。 |
| `physicaldisk.throughput.read`  | 從每秒磁碟讀取的資料數量。                            |
| `physicaldisk.throughput.write` | 每秒磁碟寫入的資料數量。                           |
| `physicaldisk.throughput.total` | 資料讀取或寫入每秒的磁碟機的總數量。        |
| `physicaldisk.latency.read`     | 從磁碟機的讀取作業的平均延遲。                          |
| `physicaldisk.latency.write`    | 磁碟機的寫入作業的平均延遲。                           |
| `physicaldisk.latency.average`  | 或從磁碟機的所有作業的平均延遲。                     |
| `physicaldisk.size.total`       | 磁碟機的總儲存容量。                                    |
| `physicaldisk.size.used`        | 磁碟機使用的儲存體容量。                                     |

## <a name="where-they-come-from"></a>各來源來自

`iops.*`， `throughput.*`，並`latency.*`系列會收集從`Physical Disk`效能計數器在伺服器上設定磁碟機連線的位置，每個磁碟機的一個執行個體。 這些計數器來測量`partmgr.sys`，且不包含最多的 Windows 軟體堆疊或任何網路躍點。 也就是效能的代表裝置硬體。

| 系列                          | 來源計數器           |
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
   > 計數器會測量整個間隔，不取樣。 例如，如果磁碟機閒置 9 秒但完成 30 IOs 10 秒，其`physicaldisk.iops.total`將為每秒的 3 個 IOs 10 秒間隔期間的平均會記錄。 這可確保其效能歷程記錄會擷取所有的活動，並是強固的雜訊。

`size.*`序列會收集從`MSFT_PhysicalDisk`在 WMI 中，每個磁碟機的一個執行個體的類別。

| 系列                          | Source 屬性        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用[Get-physicaldisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) cmdlet:

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能歷程記錄的儲存空間直接存取](performance-history.md)
