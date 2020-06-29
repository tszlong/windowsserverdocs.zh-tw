---
title: 虛擬硬碟的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 18694975e48d199f2f690aebe8af2a4613a4b1f0
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474705"
---
# <a name="performance-history-for-virtual-hard-disks"></a>虛擬硬碟的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取的效能歷程記錄](performance-history.md)的子主題會詳細說明針對虛擬硬碟（VHD）檔案收集的效能歷程記錄。 效能歷程記錄適用于連接至執行中叢集虛擬機器的每個 VHD。 效能歷程記錄適用于 VHD 和 VHDX 格式，但不適用於共用 VHDX 檔案。

   > [!NOTE]
   > 可能需要幾分鐘的時間，才能開始收集新建立或移動的 VHD 檔案。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會針對每個符合資格的虛擬硬碟收集這些系列：

| 數列                    | 單位             |
|---------------------------|------------------|
| `vhd.iops.read`           | 每秒       |
| `vhd.iops.write`          | 每秒       |
| `vhd.iops.total`          | 每秒       |
| `vhd.throughput.read`     | 每秒位元組數 |
| `vhd.throughput.write`    | 每秒位元組數 |
| `vhd.throughput.total`    | 每秒位元組數 |
| `vhd.latency.average`     | seconds          |
| `vhd.size.current`        | 位元組            |
| `vhd.size.maximum`        | 位元組            |

## <a name="how-to-interpret"></a>如何解讀

| 數列                    | 如何解讀                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | 虛擬硬碟每秒完成的讀取作業數目。                                         |
| `vhd.iops.write`          | 虛擬硬碟每秒完成的寫入作業數目。                                        |
| `vhd.iops.total`          | 虛擬硬碟每秒完成的讀取或寫入作業總數。                          |
| `vhd.throughput.read`     | 每秒從虛擬硬碟讀取的資料數量。                                                     |
| `vhd.throughput.write`    | 每秒寫入虛擬硬碟的資料數量。                                                    |
| `vhd.throughput.total`    | 從虛擬硬碟讀取或寫入的總數據量（以秒為單位）。                                 |
| `vhd.latency.average`     | 虛擬硬碟的所有作業的平均延遲。                                              |
| `vhd.size.current`        | 虛擬硬碟的目前檔案大小（如果動態擴充）。 如果已修正，則不會收集數列。 |
| `vhd.size.maximum`        | 虛擬硬碟的大小上限（如果動態擴充）。 若已修正，則為大小。                  |

## <a name="where-they-come-from"></a>來自何處

`iops.*`、 `throughput.*` 和 `latency.*` 系列是從執行 `Hyper-V Virtual Storage Device` 虛擬機器的伺服器上的效能計數器集合收集而來，每個 VHD 或 VHDX 一個實例。

| 數列                    | 來源計數器         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *上述的總和*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *上述的總和*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > 計數器是以整個間隔來測量，而不是取樣。 例如，如果 VHD 處於非使用中狀態9秒，但在第10秒內完成30個 Io，則在 `vhd.iops.total` 此10秒的間隔期間，將會平均將其記錄為每秒3個 io。 這可確保其效能歷程會捕捉所有活動，而且健全于雜訊。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) Cmdlet：

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

若要從虛擬機器取得每個 VHD 的路徑：

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > 取得 VHD Cmdlet 需要提供檔案路徑。 它不支援列舉。

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
