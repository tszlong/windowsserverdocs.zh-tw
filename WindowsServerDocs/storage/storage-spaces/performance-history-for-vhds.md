---
title: 虛擬硬碟效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589716"
---
# <a name="performance-history-for-virtual-hard-disks"></a>虛擬硬碟效能歷程記錄

> 適用於： Windows Server 內部預覽

[效能歷程記錄的儲存空間直接](performance-history.md)此子主題會說明在效能歷程記錄收集的虛擬硬碟 (VHD) 檔案的詳細資料。 使用的每一個附加至執行、 叢集虛擬機器的 VHD 效能歷程記錄。 不過，它不是可供共用 VHDX 檔案使用 VHD 和 VHDX 格式效能歷程記錄。

   > [!NOTE]
   > 可能需要幾分鐘的時間開始新建立或移動的 VHD 檔案集合。

## <a name="series-names-and-units"></a>數列名稱及單位

這些數列會針對每個合格的虛擬硬碟收集：

| 一系列                    | 單位             |
|---------------------------|------------------|
| `vhd.iops.read`           | 每秒       |
| `vhd.iops.write`          | 每秒       |
| `vhd.iops.total`          | 每秒       |
| `vhd.throughput.read`     | 每秒位元組數 |
| `vhd.throughput.write`    | 每秒位元組數 |
| `vhd.throughput.total`    | 每秒位元組數 |
| `vhd.latency.average`     | seconds          |
| `vhd.size.current`        |  位元組            |
| `vhd.size.maximum`        |  位元組            |

## <a name="how-to-interpret"></a>如何轉譯

| 一系列                    | 如何轉譯                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | 每秒完成的虛擬硬碟的讀取作業數目。                                         |
| `vhd.iops.write`          | 每秒完成的虛擬硬碟的寫入作業數目。                                        |
| `vhd.iops.total`          | 總數讀取或寫入作業每秒的虛擬硬碟來完成。                          |
| `vhd.throughput.read`     | 從虛擬硬碟磁碟每秒讀取的資料量。                                                     |
| `vhd.throughput.write`    | 資料寫入到每秒的虛擬硬碟的數量。                                                    |
| `vhd.throughput.total`    | 總資料讀取或寫入至每秒的虛擬硬碟的數量。                                 |
| `vhd.latency.average`     | 進出虛擬硬碟的所有作業的平均延遲。                                              |
| `vhd.size.current`        | 目前的檔案大小的虛擬硬碟，如果動態展開。 如果固定，不會收集數列。 |
| `vhd.size.maximum`        | 虛擬硬碟，如果動態展開的大小上限。 如果固定、 大小。                  |

## <a name="where-they-come-from"></a>他們來自的位置

`iops.*`、 `throughput.*`，及`latency.*`一系列的收集方法從`Hyper-V Virtual Storage Device`效能計數器在伺服器上設定虛擬機器所在執行、 每個 VHD 或 VHDX 一個執行個體。

| 一系列                    | 來源效能計數器         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *在上述的總和*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *在上述的總和*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > 計數器計算單位是透過整個間隔，不時。 例如，如果 VHD 為非作用中的 9 秒但完成 30 IOs 第 10 秒，其`vhd.iops.total`將為每秒 3 IOs 此 10 秒間隔期間的平均會記錄。 這可確保其效能歷程記錄擷取所有的活動和很完善至雜訊。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用狀況

使用[Get VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd)指令程式：

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

若要取得的每個 VHD 路徑從虛擬機器：

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > 取得 VHD 指令程式需要以提供檔案路徑。 不支援列舉。

## <a name="see-also"></a>另請參閱

- [效能的儲存空間直接的歷程記錄](performance-history.md)
