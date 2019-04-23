---
title: 虛擬硬碟的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880399"
---
# <a name="performance-history-for-virtual-hard-disks"></a>虛擬硬碟的效能歷程記錄

> 適用於：Windows Server Insider Preview

子主題[效能歷程記錄的儲存空間直接存取](performance-history.md)詳細說明虛擬硬碟 (VHD) 檔案所收集的效能歷程記錄。 效能歷程記錄可供每個連接至執行的叢集虛擬機器的 VHD。 效能歷程記錄可供 VHD 和 VHDX 格式，但它不提供共用 VHDX 檔案。

   > [!NOTE]
   > 可能需要幾分鐘的時間開始針對新建立或移動的 VHD 檔案的集合。

## <a name="series-names-and-units"></a>數列名稱與單位

這些數列會收集每個符合資格的虛擬硬碟：

| 系列                    | 單位             |
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

## <a name="how-to-interpret"></a>如何解譯

| 系列                    | 如何解譯                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | 每秒完成的虛擬硬碟的讀取作業數目。                                         |
| `vhd.iops.write`          | 每秒完成的虛擬硬碟的寫入作業數目。                                        |
| `vhd.iops.total`          | 讀取或寫入作業每秒完成的虛擬硬碟的總數。                          |
| `vhd.throughput.read`     | 從虛擬硬碟每秒讀取的資料數量。                                                     |
| `vhd.throughput.write`    | 虛擬硬碟每秒寫入的資料量。                                                    |
| `vhd.throughput.total`    | 資料讀取或寫入至虛擬硬碟每秒的總數量。                                 |
| `vhd.latency.average`     | 所有作業，或從虛擬硬碟的平均延遲。                                              |
| `vhd.size.current`        | 目前的檔案大小的虛擬硬碟，若動態擴充。 固定的如果數列不會進行收集。 |
| `vhd.size.maximum`        | 虛擬硬碟，若動態擴充的大小上限。 如果固定的是的大小。                  |

## <a name="where-they-come-from"></a>各來源來自

`iops.*`， `throughput.*`，並`latency.*`系列會收集從`Hyper-V Virtual Storage Device`效能計數器在伺服器上設定虛擬機器的位置執行，請針對每個 VHD 或 VHDX 的一個執行個體。

| 系列                    | 來源計數器         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *上述的總和*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *上述的總和*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > 計數器會測量整個間隔，不取樣。 比方說，如果 VHD 是達 9 秒但完成 30 IOs 10 秒，其`vhd.iops.total`將為每秒的 3 個 IOs 10 秒間隔期間的平均會記錄。 這可確保其效能歷程記錄會擷取所有的活動，並是強固的雜訊。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用[GET-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) cmdlet:

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

若要從虛擬機器取得的每個 VHD 的路徑：

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > GET-VHD cmdlet 需要提供檔案路徑。 不支援列舉型別。

## <a name="see-also"></a>另請參閱

- [效能歷程記錄的儲存空間直接存取](performance-history.md)
