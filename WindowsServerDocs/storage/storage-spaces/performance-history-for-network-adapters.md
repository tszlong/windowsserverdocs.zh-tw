---
title: 網路介面卡的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: ee8204d14ff1d54d3a4a5b1760055fecdc952d05
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957255"
---
# <a name="performance-history-for-network-adapters"></a>網路介面卡的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取的效能歷程記錄](performance-history.md)的子主題會詳細說明針對網路介面卡收集的效能歷程記錄。 網路介面卡效能歷程記錄適用于叢集中每部伺服器的每個實體網路介面卡。  (RDMA 的遠端直接記憶體存取) 效能歷程記錄可供已啟用 RDMA 的每個實體網路介面卡使用。

   > [!NOTE]
   > 無法為關閉的伺服器中的網路介面卡收集效能歷程記錄。 當伺服器恢復運作時，集合會自動繼續。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會針對每個合格的網路介面卡收集這些系列：

| 數列                               | 單位            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | 每秒位數 |
| `netadapter.bandwidth.outbound`      | 每秒位數 |
| `netadapter.bandwidth.total`         | 每秒位數 |
| `netadapter.bandwidth.rdma.inbound`  | 每秒位數 |
| `netadapter.bandwidth.rdma.outbound` | 每秒位數 |
| `netadapter.bandwidth.rdma.total`    | 每秒位數 |

   > [!NOTE]
   > 網路介面卡效能歷程記錄以每秒**位數**記錄，而不是每秒位元組。 1 10 GbE 網路介面卡每秒最多可傳送和接收1000000000位 = 125000000 bytes = 每秒 1.25 GB。

## <a name="how-to-interpret"></a>如何解讀

| 數列                               | 如何解讀                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | 網路介面卡收到的資料速率。                         |
| `netadapter.bandwidth.outbound`      | 網路介面卡所傳送的資料速率。                             |
| `netadapter.bandwidth.total`         | 網路介面卡接收或傳送的資料總速率。           |
| `netadapter.bandwidth.rdma.inbound`  | 網路介面卡透過 RDMA 接收的資料速率。               |
| `netadapter.bandwidth.rdma.outbound` | 網路介面卡透過 RDMA 傳送的資料速率。                   |
| `netadapter.bandwidth.rdma.total`    | 網路介面卡透過 RDMA 接收或傳送的資料總速率。 |

## <a name="where-they-come-from"></a>來自何處

此 `bytes.*` 系列會從 `Network Adapter` 已安裝網路介面卡的伺服器上的效能計數器集合收集，每個網路介面卡一個實例。

| 數列                           | 來源計數器           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8×`Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8×`Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8×`Bytes Total/sec`    |

此 `rdma.*` 系列會從 `RDMA Activity` 已安裝網路介面卡的伺服器上的效能計數器集合中收集，而每個網路介面卡都有一個已啟用 RDMA 的實例。

| 數列                               | 來源計數器           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8×`Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8×`Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | *上列的*8 ×總和   |

   > [!NOTE]
   > 計數器是以整個間隔來測量，而不是取樣。 例如，如果網路介面卡閒置了9秒，但在10秒內傳輸200位，則在 `netadapter.bandwidth.total` 此10秒的間隔期間，平均會將其記錄為每秒20位。 這可確保其效能歷程會捕捉所有活動，而且健全于雜訊。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[get-netadapter](/powershell/module/netadapter/get-netadapter) Cmdlet：

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
