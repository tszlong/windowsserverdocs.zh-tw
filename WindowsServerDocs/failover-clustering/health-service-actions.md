---
title: 健全狀況服務動作
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 519f0321f36fb7afc86962950aeab729d7a38adb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361105"
---
# <a name="health-service-actions"></a>健全狀況服務動作

> 適用於：Windows Server 2019、Windows Server 2016

健全狀況服務是 Windows Server 2016 中的新功能，可改善執行儲存空間直接存取之叢集的日常監視和操作體驗。

## <a name="actions"></a>動作  

下一節會說明由「健全狀況服務」自動執行的工作流程。 為了確認確實自動採取動作 (或為了追蹤其進度或結果)，「健全狀況服務」會產生「動作」。 不同於記錄檔，「動作」完成之後很快就會消失，且主要目的是針對可能會影響效能或容量的進行中活動 (例如還原復原能力或重新平衡資料) 提供深入見解。  

### <a name="usage"></a>使用量  

有一個新的 PowerShell 指令程式會顯示所有「動作」︰  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>涵蓋範圍  

在 Windows Server 2016 中， **StorageHealthAction** Cmdlet 可以傳回下列任何資訊：  

-   淘汰失敗、失去連線，或實體磁碟沒有回應  

-   正在切換儲存集區以使用取代用實體磁碟  

-   正在還原資料的完整復原能力  

-   正在重新平衡儲存集區  

## <a name="see-also"></a>另請參閱

- [Windows Server 2016 中的健全狀況服務](health-service-overview.md)
- [MSDN 上的開發人員檔、範例程式碼和 API 參考](https://msdn.microsoft.com/windowshealthservice)
