---
title: 群集的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: e013295364ae2951ffe8a963fb61a85d7863f5b6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962050"
---
# <a name="performance-history-for-clusters"></a>群集的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取的效能歷程記錄](performance-history.md)的子主題說明為叢集收集的效能歷程記錄。

沒有源自叢集層級的數列。 相反地，伺服器系列（例如 `clusternode.cpu.usage` ）會針對叢集中的所有伺服器進行匯總。 磁片區系列（例如 `volume.iops.total` ）會針對叢集中的所有磁片區進行匯總。 和磁片磁碟機系列（例如 `physicaldisk.size.total` ）會針對叢集中的所有磁片磁碟機進行匯總。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[取得](/powershell/module/failoverclusters/get-cluster)叢集 Cmdlet：

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
