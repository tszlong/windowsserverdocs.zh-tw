---
description: 深入瞭解：網路介面卡的效能歷程記錄
title: 網路介面卡的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: c47b5f2ce60a952eb8c7773284d976e6b99d1477
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040086"
---
# <a name="performance-history-for-network-adapters"></a>網路介面卡的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取效能歷程記錄](performance-history.md)的子主題詳細說明為網路介面卡收集的效能歷程記錄。 網路介面卡效能歷程記錄適用于叢集中每部伺服器上的每個實體網路介面卡。 遠端直接記憶體存取 (RDMA) 效能歷程記錄適用于已啟用 RDMA 的每個實體網路介面卡。

   > [!NOTE]
   > 無法針對關閉的伺服器中的網路介面卡收集效能歷程記錄。 當伺服器恢復運作時，集合會自動繼續。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會為每個符合資格的網路介面卡收集這些系列：

| 數列                               | 單位            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | 每秒位數 |
| `netadapter.bandwidth.outbound`      | 每秒位數 |
| `netadapter.bandwidth.total`         | 每秒位數 |
| `netadapter.bandwidth.rdma.inbound`  | 每秒位數 |
| `netadapter.bandwidth.rdma.outbound` | 每秒位數 |
| `netadapter.bandwidth.rdma.total`    | 每秒位數 |

   > [!NOTE]
   > 網路介面卡效能歷程記錄以每秒 **位數** （而不是每秒位元組數）記錄。 1 10 GbE 網路介面卡每秒可傳送和接收大約1000000000個位 = 125000000 bytes = 每秒 1.25 GB。

## <a name="how-to-interpret"></a>如何解讀

| 數列                               | 如何解讀                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | 網路介面卡接收資料的速率。                         |
| `netadapter.bandwidth.outbound`      | 網路介面卡傳送的資料速率。                             |
| `netadapter.bandwidth.total`         | 網路介面卡接收或傳送的資料總速率。           |
| `netadapter.bandwidth.rdma.inbound`  | 網路介面卡透過 RDMA 接收資料的速率。               |
| `netadapter.bandwidth.rdma.outbound` | 網路介面卡透過 RDMA 傳送資料的速率。                   |
| `netadapter.bandwidth.rdma.total`    | 網路介面卡透過 RDMA 接收或傳送的資料總速率。 |

## <a name="where-they-come-from"></a>來源

`bytes.*`系統會從 `Network Adapter` 安裝網路介面卡的伺服器上的效能計數器集合收集系列，每張網路介面卡一個實例。

| 數列                           | Source 計數器           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8× `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8× `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8× `Bytes Total/sec`    |

`rdma.*`系統會從 `RDMA Activity` 安裝網路介面卡的伺服器上的效能計數器集合中收集此系列，並啟用 RDMA 的每個網路介面卡都有一個實例。

| 數列                               | Source 計數器           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8× `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8× `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | *上述的* 8 ×總和   |

   > [!NOTE]
   > 計數器會以整個間隔來測量，而不會進行取樣。 例如，如果網路介面卡閒置9秒，但在10秒內傳輸200位，則在 `netadapter.bandwidth.total` 此10秒的間隔期間，每秒會以20位的平均記錄。 這樣可確保其效能歷程記錄會捕捉所有活動，而且對雜訊而言很健全。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用 [get-netadapter](/powershell/module/netadapter/get-netadapter) 指令 Cmdlet：

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
