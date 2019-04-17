---
title: 伺服器效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894247"
---
# <a name="performance-history-for-servers"></a>伺服器效能歷程記錄

> 適用於： Windows Server 內部預覽

[效能歷程記錄的儲存空間直接](performance-history.md)此子主題會說明在伺服器收集的效能歷程記錄的詳細資料。 使用叢集中的每個伺服器的效能歷程記錄。

   > [!NOTE]
   > 效能歷程記錄不能收集的已關閉的伺服器。 集合將會繼續自動時伺服器而言 back up。

## <a name="series-names-and-units"></a>數列名稱及單位

這些數列收集合格的每部伺服器：

| 一系列                           | 單位    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | % |
| `clusternode.cpu.usage.guest`    | % |
| `clusternode.cpu.usage.host`     | % |
| `clusternode.memory.total`       |  位元組   |
| `clusternode.memory.available`   |  位元組   |
| `clusternode.memory.usage`       |  位元組   |
| `clusternode.memory.usage.guest` |  位元組   |
| `clusternode.memory.usage.host`  |  位元組   |

此外，例如磁碟機數列`physicaldisk.size.total`彙總的所有合格的磁碟機附加至伺服器及網路介面卡數列例如`networkadapter.bytes.total`會附加至伺服器的所有合格的網路介面卡彙總。

## <a name="how-to-interpret"></a>如何轉譯

| 一系列                           | 如何轉譯                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | 未閒置的處理器時間百分比。                        |
| `clusternode.cpu.usage.guest`    | 用於客體 （虛擬機器） 需求的處理器時間百分比。 |
| `clusternode.cpu.usage.host`     | 用於主機需求的處理器時間百分比。                    |
| `clusternode.memory.total`       | 伺服器的實體記憶體總量。                              |
| `clusternode.memory.available`   | 伺服器可用的記憶體。                                   |
| `clusternode.memory.usage`       | 伺服器的配置 （不適用） 的記憶體。                   |
| `clusternode.memory.usage.guest` | 記憶體配置給客體 （虛擬機器） 需求。               |
| `clusternode.memory.usage.host`  | 記憶體配置給主機需求。                                  |

## <a name="where-they-come-from"></a>他們來自的位置

`cpu.*`一系列的收集方法從根據是否啟用 HYPER-V 的不同的效能計數器。

啟用 HYPER-V 時：

| 一系列                           | 來源效能計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

使用`% Total Run Time`計數器可確保效能歷程記錄屬性所有使用方式。

如果未啟用 HYPER-V:

| 一系列                           | 來源效能計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *零* |
| `clusternode.cpu.usage.host`     | *相同總使用量* |

明文完美同步處理、`clusternode.cpu.usage`一定是`clusternode.cpu.usage.host`加上`clusternode.cpu.usage.guest`。

使用相同的警告，`clusternode.cpu.usage.guest`一定是總和`vm.cpu.usage`之主機伺服器上的所有虛擬機器。

`memory.*`系列是 （推出傳入）。

  > [!NOTE]
  > 計數器計算單位是透過整個間隔，不時。 例如，如果伺服器處於閒置 9 秒但為 100 %cpu 暴增第 10 秒，其`clusternode.cpu.usage`將 10%為此 10 秒間隔期間的平均會記錄。 這可確保其效能歷程記錄擷取所有的活動和很完善至雜訊。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用狀況

使用[Get-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode)指令程式：

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能的儲存空間直接的歷程記錄](performance-history.md)
