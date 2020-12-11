---
description: 深入瞭解：虛擬硬碟的效能歷程記錄
title: 虛擬硬碟的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: ff9990ce720330e63679b89ed63cbcc01fc2c57f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040056"
---
# <a name="performance-history-for-virtual-hard-disks"></a>虛擬硬碟的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取效能歷程記錄](performance-history.md)的子主題詳細說明為虛擬硬碟 (VHD) 檔收集的效能歷程記錄。 每個連接到執行中的叢集虛擬機器的 VHD 都可使用效能歷程記錄。 效能歷程記錄適用于 VHD 和 VHDX 格式，但不能用於共用 VHDX 檔案。

   > [!NOTE]
   > 收集可能需要幾分鐘的時間，才會開始新建立或移動的 VHD 檔案。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會為每個符合資格的虛擬硬碟收集這些系列：

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
| `vhd.iops.read`           | 虛擬硬碟每秒完成的讀取作業數。                                         |
| `vhd.iops.write`          | 虛擬硬碟每秒完成的寫入作業數。                                        |
| `vhd.iops.total`          | 虛擬硬碟每秒完成的讀取或寫入作業總數。                          |
| `vhd.throughput.read`     | 每秒從虛擬硬碟讀取的資料量。                                                     |
| `vhd.throughput.write`    | 每秒寫入虛擬硬碟的資料數量。                                                    |
| `vhd.throughput.total`    | 每秒從虛擬硬碟讀取或寫入的總數據量。                                 |
| `vhd.latency.average`     | 虛擬硬碟所有作業的平均延遲。                                              |
| `vhd.size.current`        | 虛擬硬碟的目前檔案大小（如果動態擴充）。 如果已修正，則不會收集數列。 |
| `vhd.size.maximum`        | 虛擬硬碟的大小上限（若以動態方式擴充）。 如果是固定的，則是大小。                  |

## <a name="where-they-come-from"></a>來源

`iops.*`、 `throughput.*` 和 `latency.*` 系列收集自虛擬機器執行所在 `Hyper-V Virtual Storage Device` 伺服器上的效能計數器集合，每個 VHD 或 VHDX 一個實例。

| 數列                    | Source 計數器         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *上述的總和*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *上述的總和*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > 計數器會以整個間隔來測量，而不會進行取樣。 例如，如果 VHD 處於非使用中狀態9秒，但在10秒內完成了30個 IOs，則 `vhd.iops.total` 在此10秒的間隔期間，每秒會記錄為3個 io。 這樣可確保其效能歷程記錄會捕捉所有活動，而且對雜訊而言很健全。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用「 [取得 VHD](/powershell/module/hyper-v/get-vhd) 」 Cmdlet：

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

若要從虛擬機器取得每個 VHD 的路徑：

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > 取得 VHD Cmdlet 需要提供檔案路徑。 它不支援列舉。

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
