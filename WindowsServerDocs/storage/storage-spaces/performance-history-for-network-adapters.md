---
title: 網路介面卡效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849979"
---
# <a name="performance-history-for-network-adapters"></a>網路介面卡效能歷程記錄

> 適用於：Windows Server Insider Preview

子主題[效能歷程記錄的儲存空間直接存取](performance-history.md)詳細說明收集網路介面卡的效能歷程記錄。 網路介面卡效能歷程，可供叢集中的每一部伺服器中的每個實體網路介面卡。 遠端直接記憶體存取 (RDMA) 效能歷程記錄可供每個實體網路介面卡啟用 rdma。

   > [!NOTE]
   > 無法收集效能歷程記錄已關閉的伺服器中的網路介面卡。 集合將會自動繼續時伺服器恢復運作。

## <a name="series-names-and-units"></a>數列名稱與單位

這些數列會收集每個符合資格的網路介面卡：

| 系列                               | 單位            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | 每秒位元 |
| `netadapter.bandwidth.outbound`      | 每秒位元 |
| `netadapter.bandwidth.total`         | 每秒位元 |
| `netadapter.bandwidth.rdma.inbound`  | 每秒位元 |
| `netadapter.bandwidth.rdma.outbound` | 每秒位元 |
| `netadapter.bandwidth.rdma.total`    | 每秒位元 |

   > [!NOTE]
   > 網路介面卡效能歷程記錄在**位元**每秒，而不是每秒位元組。 其中一個 10 GbE 網路介面卡可以傳送和接收大約 1,000,000,000 的位元 = 125,000,000 位元組 = 1.25 GB 每秒在其理論的最大值。

## <a name="how-to-interpret"></a>如何解譯

| 系列                               | 如何解譯                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | 網路介面卡接收資料的速率。                         |
| `netadapter.bandwidth.outbound`      | 網路介面卡所傳送的資料速率。                             |
| `netadapter.bandwidth.total`         | 總資料的速率所接收或傳送的網路介面卡。           |
| `netadapter.bandwidth.rdma.inbound`  | 透過 RDMA 網路介面卡所接收的資料的速率。               |
| `netadapter.bandwidth.rdma.outbound` | RDMA 網路介面卡所傳送的資料的速率。                   |
| `netadapter.bandwidth.rdma.total`    | 總資料的速率接收，或透過 RDMA 網路介面卡所傳送。 |

## <a name="where-they-come-from"></a>各來源來自

`bytes.*`序列會收集從`Network Adapter`設定在伺服器上已安裝的網路介面卡的效能計數器、 網路介面卡每一個執行個體。

| 系列                           | 來源計數器           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

`rdma.*`序列會收集從`RDMA Activity`設定在伺服器上已安裝的網路介面卡的效能計數器、 啟用 RDMA 的網路介面卡每一個執行個體。

| 系列                               | 來源計數器           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 ×*上述的總和*   |

   > [!NOTE]
   > 計數器會測量整個間隔，不取樣。 例如，如果網路介面卡閒置 9 秒，但傳輸 200 位元在 10 秒，其`netadapter.bandwidth.total`會記錄為 20 位元 / 秒上平均 10 秒間隔期間。 這可確保其效能歷程記錄會擷取所有的活動，並是強固的雜訊。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用[Get-netadapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) cmdlet:

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能歷程記錄的儲存空間直接存取](performance-history.md)
