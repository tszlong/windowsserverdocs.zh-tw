---
ms.assetid: f15c02d7-1cbd-4eba-a571-0ea34ab93ef4
title: 執行重複資料刪除
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 0421faaa910a1d679d809b88c0b4d2c94ba694b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852469"
---
# <a name="running-data-deduplication"></a>執行重複資料刪除

> 適用於：Windows Server （半年通道），Windows Server 2016

## <a id="running-dedup-jobs-manually"></a>手動執行重複資料刪除工作

您可以使用下列 PowerShell Cmdlet，手動執行每個排程的重複資料刪除工作：
* [`Start-DedupJob`](https://technet.microsoft.com/library/hh848442.aspx):啟動新的重複資料刪除工作
* [`Stop-DedupJob`](https://technet.microsoft.com/library/hh848439.aspx):停止已在進行中的重複資料刪除工作 （或將它從佇列移除）
* [`Get-DedupJob`](https://technet.microsoft.com/library/hh848452.aspx):顯示所有作用中和已排入佇列重複資料刪除工作

所有[當您在排程重複資料刪除工作時可使用的設定](advanced-settings.md#modifying-job-schedules-available-settings)，也可以在您手動啟動工作 (排程特有的設定除外) 時使用。 例如，若要手動啟動具有高優先順序、最大 CPU 使用量，以及最大記憶體使用量的[最佳化](understand.md#job-info-optimization)工作，請以系統管理員權限執行下列 PowerShell 命令：

```PowerShell
Start-DedupJob -Type Optimization -Volume <Your-Volume-Here> -Memory 100 -Cores 100 -Priority High
```

## <a id="monitoring-dedup"></a>監視重複資料刪除

### <a id="monitoring-dedup-job-successes"></a>工作成功

由於重複資料刪除會使用後續處理模型，請務必確認[重複資料刪除工作](understand.md#job-info)會成功。 用來檢查最新工作狀態的簡單方法是使用 [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx) PowerShell Cmdlet。 定期檢查下列欄位：

* 針對[最佳化工作](understand.md#job-info-optimization)，查看 `LastOptimizationResult` (0 = 成功)、`LastOptimizationResultMessage`，及 `LastOptimizationTime` (應該是最新的)。
* 針對[記憶體回收工作](understand.md#job-info-gc)，查看 `LastGarbageCollectionResult` (0 = 成功)、`LastGarbageCollectionResultMessage`，及 `LastGarbageCollectionTime` (應該是最新的)。
* 針對[清除工作](understand.md#job-info-scrubbing)，查看 `LastScrubbingResult` (0 = 成功)、`LastScrubbingResultMessage`，及 `LastScrubbingTime` (應該是最新的)。

> [!Note]  
> 如需工作成功和失敗的其他詳細資料，請參閱 `\Applications and Services Logs\Windows\Deduplication\Operational` 下的 Windows 事件檢視器。

### <a id="monitoring-dedup-optimization-rates"></a>最佳化比率

[最佳化工作](understand.md#job-info-optimization)失敗的一個指標是最佳化比率有向下趨勢，這可能表示最佳化工作跟不上變更或變換的速率。 您可以使用 [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx) PowerShell Cmdlet 來檢查最佳化比率。

> [!Important]  
> `Get-DedupStatus` 有兩個欄位與最佳化比率相關：`OptimizedFilesSavingsRate`和`SavingsRate`。 這兩者都是需要追蹤的重要值，但每一個都具有獨特的意義。
- `OptimizedFilesSavingsRate` 僅適用於 「 原則 」 的檔案進行最佳化 (`space used by optimized files after optimization / logical size of optimized files`)。
- `SavingsRate` 適用於整個磁碟區 (`space used by optimized files after optimization / total logical size of the optimization`)。

## <a id="disabling-dedup"></a>停用重複資料刪除
若要關閉重複資料刪除，請執行[取消最佳化工作](understand.md#job-info-unoptimization)。 若要復原磁碟區最佳化，請執行下列命令：

```PowerShell
Start-DedupJob -Type Unoptimization -Volume <Desired-Volume>
```

> [!Important]  
> 如果磁碟區沒有足夠空間來保存已取消最佳化的資料，則取消最佳化工作將會失敗。

## <a id="faq"></a>常見問題集
**是否有 System Center Operations Manager 管理組件可用來監視重複資料刪除？**  
是的。 重複資料刪除可透過檔案伺服器的 System Center 管理組件來監視。 如需詳細資訊，請參閱[檔案伺服器 2012 R2 的 System Center 管理組件指南](https://download.microsoft.com/download/6/F/7/6F7A33B9-9383-48ED-9252-23C2C8AD1BDA/MPGuide_FileServer2012R2.doc)文件。
