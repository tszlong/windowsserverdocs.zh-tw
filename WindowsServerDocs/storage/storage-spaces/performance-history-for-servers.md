---
title: 伺服器的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 262c35e7d91fe43d2ade955d4606279f8d428f6a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946331"
---
# <a name="performance-history-for-servers"></a>伺服器的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取的效能歷程記錄](performance-history.md)的子主題詳細說明為伺服器收集的效能歷程記錄。 效能歷程記錄適用于叢集中的每一部伺服器。

   > [!NOTE]
   > 無法為關閉的伺服器收集效能歷程記錄。 當伺服器恢復運作時，集合會自動繼續。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會針對每個合格的伺服器收集這些系列：

| 數列                           | 單位    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | percent |
| `clusternode.cpu.usage.guest`    | percent |
| `clusternode.cpu.usage.host`     | percent |
| `clusternode.memory.total`       | 位元組   |
| `clusternode.memory.available`   | 位元組   |
| `clusternode.memory.usage`       | 位元組   |
| `clusternode.memory.usage.guest` | 位元組   |
| `clusternode.memory.usage.host`  | 位元組   |

此外，磁片磁碟機系列（例如） `physicaldisk.size.total` 會針對所有附加至伺服器的合格磁片磁碟機進行匯總，而網路介面卡系列（例如） `networkadapter.bytes.total` 會針對附加至伺服器的所有合格網路介面卡進行匯總。

## <a name="how-to-interpret"></a>如何解讀

| 數列                           | 如何解讀                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | 非閒置的處理器時間百分比。                        |
| `clusternode.cpu.usage.guest`    | 用於來賓 (虛擬機器) 需求的處理器時間百分比。 |
| `clusternode.cpu.usage.host`     | 用於主機需求的處理器時間百分比。                    |
| `clusternode.memory.total`       | 伺服器的實體記憶體總計。                              |
| `clusternode.memory.available`   | 伺服器的可用記憶體。                                   |
| `clusternode.memory.usage`       | 配置的 (無法) 伺服器記憶體使用。                   |
| `clusternode.memory.usage.guest` | 配置給來賓 (虛擬機器) 要求的記憶體。               |
| `clusternode.memory.usage.host`  | 配置給主機需求的記憶體。                                  |

## <a name="where-they-come-from"></a>來自何處

`cpu.*`系統會根據是否啟用 hyper-v，從不同的效能計數器收集數列。

如果已啟用 Hyper-v：

| 數列                           | 來源計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

使用 `% Total Run Time` 計數器可確保效能歷程記錄屬性的所有使用方式。

如果未啟用 Hyper-v：

| 數列                           | 來源計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *零* |
| `clusternode.cpu.usage.host`     | *與總使用量相同* |

儘管是完美同步處理， `clusternode.cpu.usage` 一律會 `clusternode.cpu.usage.host` 加上 `clusternode.cpu.usage.guest` 。

有同樣的警告， `clusternode.cpu.usage.guest` 一律是 `vm.cpu.usage` 主機伺服器上所有虛擬機器的總和。

此 `memory.*` 系列 (即將推出) 。

  > [!NOTE]
  > 計數器是以整個間隔來測量，而不是取樣。 例如，如果伺服器閒置9秒，但尖峰到 100% CPU 的第10秒，則在 `clusternode.cpu.usage` 此10秒的間隔期間，平均會將其記錄為10%。 這可確保其效能歷程會捕捉所有活動，而且健全于雜訊。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[start-clusternode](/powershell/module/failoverclusters/get-clusternode) Cmdlet：

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
