---
title: 效能歷程記錄伺服器
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820589"
---
# <a name="performance-history-for-servers"></a>效能歷程記錄伺服器

> 適用於：Windows Server Insider Preview

子主題[效能歷程記錄的儲存空間直接存取](performance-history.md)詳細說明伺服器收集的效能歷程記錄。 效能歷程記錄可供叢集中的每一部伺服器。

   > [!NOTE]
   > 伺服器已關閉，無法收集效能歷程記錄。 集合將會自動繼續時伺服器恢復運作。

## <a name="series-names-and-units"></a>數列名稱與單位

這些數列會收集每個符合資格的伺服器：

| 系列                           | 單位    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | 百分比 |
| `clusternode.cpu.usage.guest`    | 百分比 |
| `clusternode.cpu.usage.host`     | 百分比 |
| `clusternode.memory.total`       | 位元組   |
| `clusternode.memory.available`   | 位元組   |
| `clusternode.memory.usage`       | 位元組   |
| `clusternode.memory.usage.guest` | 位元組   |
| `clusternode.memory.usage.host`  | 位元組   |

此外，例如磁碟機數列`physicaldisk.size.total`針對所有適合的磁碟機連接到伺服器，例如網路介面卡數列彙總`networkadapter.bytes.total`會彙總所有符合資格的網路介面卡連接到伺服器。

## <a name="how-to-interpret"></a>如何解譯

| 系列                           | 如何解譯                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | 不是閒置的處理器時間百分比。                        |
| `clusternode.cpu.usage.guest`    | 用於客體 （虛擬機器） 需求的處理器時間百分比。 |
| `clusternode.cpu.usage.host`     | 使用主應用程式需求的處理器時間百分比。                    |
| `clusternode.memory.total`       | 伺服器的實體記憶體總數。                              |
| `clusternode.memory.available`   | 伺服器可用的記憶體。                                   |
| `clusternode.memory.usage`       | 伺服器的配置 （無法使用） 的記憶體。                   |
| `clusternode.memory.usage.guest` | 客體 （虛擬機器） 的需求配置的記憶體。               |
| `clusternode.memory.usage.host`  | 配置給主機要求的記憶體。                                  |

## <a name="where-they-come-from"></a>各來源來自

`cpu.*`系列會收集從不同的效能計數器，會根據是否啟用 HYPER-V。

如果已啟用 HYPER-V:

| 系列                           | 來源計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

使用`% Total Run Time`計數器，可確保 「 效能歷程記錄屬性的所有使用量。

如果未啟用 HYPER-V:

| 系列                           | 來源計數器 |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *總使用量相同* |

完美的同步處理，儘管`clusternode.cpu.usage`總是`clusternode.cpu.usage.host`加上`clusternode.cpu.usage.guest`。

使用相同的警告，`clusternode.cpu.usage.guest`一律為總和`vm.cpu.usage`主機伺服器上的所有虛擬機器。

`memory.*`系列是 （即將推出）。

  > [!NOTE]
  > 計數器會測量整個間隔，不取樣。 例如，如果伺服器閒置 9 秒，但暴增到 100 %cpu 在 10 秒，其`clusternode.cpu.usage`會記錄為 10%平均 10 秒間隔期間。 這可確保其效能歷程記錄會擷取所有的活動，並是強固的雜訊。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用[Get-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) cmdlet:

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能歷程記錄的儲存空間直接存取](performance-history.md)
