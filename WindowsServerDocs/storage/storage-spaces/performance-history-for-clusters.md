---
title: 群集的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 8ee2e85723cc2449e8cb9c42ccb7d6b761482e3a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474865"
---
# <a name="performance-history-for-clusters"></a>群集的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取的效能歷程記錄](performance-history.md)的子主題說明為叢集收集的效能歷程記錄。

沒有源自叢集層級的數列。 相反地，伺服器系列（例如 `clusternode.cpu.usage` ）會針對叢集中的所有伺服器進行匯總。 磁片區系列（例如 `volume.iops.total` ）會針對叢集中的所有磁片區進行匯總。 和磁片磁碟機系列（例如 `physicaldisk.size.total` ）會針對叢集中的所有磁片磁碟機進行匯總。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[取得](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster)叢集 Cmdlet：

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
