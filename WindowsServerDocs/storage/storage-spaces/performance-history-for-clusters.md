---
title: 叢集的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894317"
---
# <a name="performance-history-for-clusters"></a>叢集的效能歷程記錄

> 適用於： Windows Server 內部預覽

[效能歷程記錄的儲存空間直接](performance-history.md)這個子主題會說明叢集收集的效能歷程記錄。

有任何源自叢集層級的數列。 而 server 系列，例如`clusternode.cpu.usage`，在叢集中的所有伺服器的彙總。 大量數列例如`volume.iops.total`，在叢集中的所有磁碟區彙總。 例如磁碟機數列和`physicaldisk.size.total`，會針對所有的磁碟機叢集中的彙總。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用狀況

使用[Get 叢集](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster)指令程式：

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>另請參閱

- [效能的儲存空間直接的歷程記錄](performance-history.md)
