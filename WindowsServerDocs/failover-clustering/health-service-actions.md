---
title: "健康服務動作"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-actions"></a>健康服務動作

> 適用於 Windows Server 2016

健康服務是 Windows Server 2016，可改善您的日常監視和操作叢集執行儲存空間直接存取體驗中的新功能。

## <a name="actions"></a>控制項目  

告訴您工作流程自動化 Health 服務的下一節。 若要檢查動作確實執行獨立，或追蹤其進行或結果，Health 服務會產生 [動作]。 然而登時，它們完成後，並主要為了提供深入了解進行的活動，可能會影響效能或容量 （例如還原恢復或平衡資料） 之後，很快就會消失動作。  

### <a name="usage"></a>使用  

一個新 PowerShell cmdlet 會顯示的所有動作：  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>涵蓋範圍  

Windows Server 2016 中**取得-StorageHealthAction** cmdlet 可以退還任何下列資訊：  

-   連接失敗，已遺失或無法回應時應實體磁碟淘汰  

-   使用替換實體磁碟切換儲存集區  

-   還原恢復功能完整的資料  

-   平衡儲存集區  

## <a name="see-also"></a>也了

- [Windows Server 2016 中 health 服務](health-service-overview.md)
- [開發人員的文件、 範例程式碼和 msdn API 參考](https://msdn.microsoft.com/windowshealthservice)
