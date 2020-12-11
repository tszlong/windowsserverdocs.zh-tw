---
description: 深入瞭解：伺服器的效能歷程記錄
title: 伺服器的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 1d932576351064d1a4b11031d4faef0e0efe7737
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041806"
---
# <a name="performance-history-for-servers"></a>伺服器的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取效能歷程記錄](performance-history.md)的子主題詳細說明針對伺服器所收集的效能歷程記錄。 效能歷程記錄適用于叢集中的每一部伺服器。

   > [!NOTE]
   > 無法針對關閉的伺服器收集效能歷程記錄。 當伺服器恢復運作時，集合會自動繼續。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會為每個合格的伺服器收集這些系列：

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

此外，磁片磁碟機系列（例如） `physicaldisk.size.total` 會針對附加至伺服器的所有合格磁片磁碟機進行匯總，而網路介面卡系列（例如） `networkadapter.bytes.total` 會針對附加至伺服器的所有符合資格的網路介面卡進行匯總。

## <a name="how-to-interpret"></a>如何解讀

| 數列                           | 如何解讀                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | 未閒置的處理器時間百分比。                        |
| `clusternode.cpu.usage.guest`    | 用於來賓 (虛擬機器) 需求的處理器時間百分比。 |
| `clusternode.cpu.usage.host`     | 用於主機需求的處理器時間百分比。                    |
| `clusternode.memory.total`       | 伺服器的總實體記憶體。                              |
| `clusternode.memory.available`   | 伺服器的可用記憶體。                                   |
| `clusternode.memory.usage`       | 配置的 (無法) 的伺服器記憶體使用。                   |
| `clusternode.memory.usage.guest` | 配置給來賓 (虛擬機器) 需求的記憶體。               |
| `clusternode.memory.usage.host`  | 配置給主機需求的記憶體。                                  |

## <a name="where-they-come-from"></a>來源

`cpu.*`系統會根據 hyper-v 是否啟用，從不同的效能計數器收集數列。

如果已啟用 Hyper-v：

| 數列                           | Source 計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

使用 `% Total Run Time` 計數器可確保效能歷程記錄屬性的所有使用方式。

如果未啟用 Hyper-v：

| 數列                           | Source 計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *零* |
| `clusternode.cpu.usage.host`     | *與總使用量相同* |

儘管完美同步處理， `clusternode.cpu.usage` 一律會 `clusternode.cpu.usage.host` 加上 `clusternode.cpu.usage.guest` 。

使用相同的警告， `clusternode.cpu.usage.guest` 一律是 `vm.cpu.usage` 主機伺服器上所有虛擬機器的總和。

此 `memory.*` 系列 (即將推出) 。

  > [!NOTE]
  > 計數器會以整個間隔來測量，而不會進行取樣。 例如，如果伺服器閒置9秒，但在10秒內尖峰至100% 的 CPU，則在 `clusternode.cpu.usage` 此10秒的間隔期間，其平均會記錄為10%。 這樣可確保其效能歷程記錄會捕捉所有活動，而且對雜訊而言很健全。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用 [get-clusternode](/powershell/module/failoverclusters/get-clusternode) 指令 Cmdlet：

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
