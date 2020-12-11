---
description: 深入瞭解：執行重復資料刪除
ms.assetid: f15c02d7-1cbd-4eba-a571-0ea34ab93ef4
title: 執行重複資料刪除
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 55d5d3ee542ed0c16f66d9e882ae5d6dafd1af99
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040346"
---
# <a name="running-data-deduplication"></a>執行重複資料刪除

> 適用於：Windows Server (半年度管道)、Windows Server 2016

## <a name="running-data-deduplication-jobs-manually"></a><a id="running-dedup-jobs-manually"></a>手動執行重復資料刪除作業

您可以使用下列 PowerShell Cmdlet，手動執行每個排程的重複資料刪除工作：
* [`Start-DedupJob`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12))：啟動新的重復資料刪除作業
* [`Stop-DedupJob`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12))：停止已在進行中的重復資料刪除工作 (或將它從佇列中移除) 
* [`Get-DedupJob`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12))：顯示所有作用中和已排入佇列的重復資料刪除作業

所有[當您在排程重複資料刪除工作時可使用的設定](advanced-settings.md#modifying-job-schedules-available-settings)，也可以在您手動啟動工作 (排程特有的設定除外) 時使用。 例如，若要手動啟動具有高優先順序、最大 CPU 使用量，以及最大記憶體使用量的[最佳化](understand.md#job-info-optimization)工作，請以系統管理員權限執行下列 PowerShell 命令：

```PowerShell
Start-DedupJob -Type Optimization -Volume <Your-Volume-Here> -Memory 100 -Cores 100 -Priority High
```

## <a name="monitoring-data-deduplication"></a><a id="monitoring-dedup"></a>監視重複資料刪除

### <a name="job-successes"></a><a id="monitoring-dedup-job-successes"></a>作業成功

由於重複資料刪除會使用後續處理模型，請務必確認[重複資料刪除工作](understand.md#job-info)會成功。 檢查最新作業狀態的簡單方法是使用 [`Get-DedupStatus`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) PowerShell Cmdlet。 定期檢查下列欄位：

* 針對[最佳化工作](understand.md#job-info-optimization)，查看 `LastOptimizationResult` (0 = 成功)、`LastOptimizationResultMessage`，及 `LastOptimizationTime` (應該是最新的)。
* 針對[記憶體回收工作](understand.md#job-info-gc)，查看 `LastGarbageCollectionResult` (0 = 成功)、`LastGarbageCollectionResultMessage`，及 `LastGarbageCollectionTime` (應該是最新的)。
* 針對[清除工作](understand.md#job-info-scrubbing)，查看 `LastScrubbingResult` (0 = 成功)、`LastScrubbingResultMessage`，及 `LastScrubbingTime` (應該是最新的)。

> [!Note]
> 如需工作成功和失敗的其他詳細資料，請參閱 `\Applications and Services Logs\Windows\Deduplication\Operational` 下的 Windows 事件檢視器。

### <a name="optimization-rates"></a><a id="monitoring-dedup-optimization-rates"></a>優化速率

[最佳化工作](understand.md#job-info-optimization)失敗的一個指標是最佳化比率有向下趨勢，這可能表示最佳化工作跟不上變更或變換的速率。 您可以使用 PowerShell Cmdlet 來檢查優化速率 [`Get-DedupStatus`](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) 。

> [!Important]
> `Get-DedupStatus` 有兩個與優化速率相關的欄位： `OptimizedFilesSavingsRate` 和 `SavingsRate` 。 這兩者都是需要追蹤的重要值，但每一個都具有獨特的意義。
> - `OptimizedFilesSavingsRate` 只適用于「同原則」 () 的優化檔案 `space used by optimized files after optimization / logical size of optimized files` 。
> - `SavingsRate` 適用于整個磁片區 (`space used by optimized files after optimization / total logical size of the optimization`) 。

## <a name="disabling-data-deduplication"></a><a id="disabling-dedup"></a>停用重複資料刪除
若要關閉重複資料刪除，請執行[取消最佳化工作](understand.md#job-info-unoptimization)。 若要復原磁碟區最佳化，請執行下列命令：

```PowerShell
Start-DedupJob -Type Unoptimization -Volume <Desired-Volume>
```

> [!Important]
> 如果磁碟區沒有足夠空間來保存已取消最佳化的資料，則取消最佳化工作將會失敗。

## <a name="frequently-asked-questions"></a><a id="faq"></a>常見問題
**是否有 System Center Operations Manager 管理組件可用來監視重複資料刪除？**
是。 重複資料刪除可透過檔案伺服器的 System Center 管理組件來監視。 如需詳細資訊，請參閱[檔案伺服器 2012 R2 的 System Center 管理組件指南](https://download.microsoft.com/download/6/F/7/6F7A33B9-9383-48ED-9252-23C2C8AD1BDA/MPGuide_FileServer2012R2.doc)文件。
