---
title: 網路介面卡的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239235"
---
# 網路介面卡的效能歷程記錄

> 適用於： Windows Server Insider Preview

[儲存空間直接存取的效能記錄](performance-history.md)的此子主題描述中所收集網路介面卡的效能歷程記錄的詳細資料。 網路介面卡的效能記錄只適用於每個叢集中的每個伺服器中的實體網路介面卡。 遠端直接記憶體存取 (RDMA) 效能歷程記錄啟用 RDMA 是適用於每個實體網路介面卡。

   > [!NOTE]
   > 無法收集效能歷程記錄中的伺服器會向下的網路介面卡。 集合會自動繼續執行時，伺服器恢復。

## 系列名稱與單位

這些一系列的收集每個符合資格的網路介面卡：

| 系列                               | 單位            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | 每秒的位元 |
| `netadapter.bandwidth.outbound`      | 每秒的位元 |
| `netadapter.bandwidth.total`         | 每秒的位元 |
| `netadapter.bandwidth.rdma.inbound`  | 每秒的位元 |
| `netadapter.bandwidth.rdma.outbound` | 每秒的位元 |
| `netadapter.bandwidth.rdma.total`    | 每秒的位元 |

   > [!NOTE]
   > 網路介面卡效能歷程記錄會記錄在**位元**不每秒位元組數秒。 其中一個 10 GbE 網路介面卡可以傳送和接收大約 1,000,000,000 位元 = 125,000,000 位元組 = 1.25 GB 每秒在其理論上的最大值。

## 如何解譯

| 系列                               | 如何解譯                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | 接收到的網路介面卡的資料的速率。                         |
| `netadapter.bandwidth.outbound`      | 網路介面卡所傳送的資料速率。                             |
| `netadapter.bandwidth.total`         | 總資料速率接收，或傳送的網路介面卡。           |
| `netadapter.bandwidth.rdma.inbound`  | 透過 RDMA 網路介面卡接收到的資料的速率。               |
| `netadapter.bandwidth.rdma.outbound` | 傳送透過 RDMA 網路介面卡的資料速率。                   |
| `netadapter.bandwidth.rdma.total`    | 收到或 RDMA 網路介面卡上傳送的資料的總比率。 |

## 他們來自何處

`bytes.*`從所收集的一系列`Network Adapter`在伺服器上設定的網路介面卡的安裝位置的效能計數器、 網路介面卡每一個執行個體。

| 系列                           | 來源計數器           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

`rdma.*`從所收集的一系列`RDMA Activity`在伺服器上設定的網路介面卡的安裝位置的效能計數器、 啟用 RDMA 的網路介面卡每一個執行個體。

| 系列                               | 來源計數器           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 ×*上述的加總*   |

   > [!NOTE]
   > 計數器是透過整個間隔，不取樣測量。 例如，如果您的網路介面卡處於閒置 9 秒，但傳輸 200 的位元 10 日的第二個，其`netadapter.bandwidth.total`會記錄為 20 秒的位元平均 10 秒的時間間隔內。 這樣可確保其效能歷程記錄會擷取所有活動，並以雜訊強固。

## 在 PowerShell 中的使用方式

使用[Get-netadapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) cmdlet:

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## 請參閱

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
