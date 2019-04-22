---
title: 叢集的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818769"
---
# <a name="performance-history-for-clusters"></a>叢集的效能歷程記錄

> 適用於：Windows Server Insider Preview

子主題[效能歷程記錄的儲存空間直接存取](performance-history.md)說明叢集所收集的效能歷程記錄。

有源自於叢集層級沒有數列。 相反地，server 系列，例如`clusternode.cpu.usage`，針對叢集中的所有伺服器彙總。 磁碟區的數列，例如`volume.iops.total`，針對叢集中的所有磁碟區彙總。 這類磁碟機系列和`physicaldisk.size.total`，會彙總的叢集中的所有磁碟機。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用[取得叢集](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster)cmdlet:

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能歷程記錄的儲存空間直接存取](performance-history.md)
