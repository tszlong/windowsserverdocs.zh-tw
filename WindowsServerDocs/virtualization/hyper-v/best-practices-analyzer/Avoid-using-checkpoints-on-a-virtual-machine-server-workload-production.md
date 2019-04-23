---
title: 請避免在生產環境中執行的伺服器工作負載的虛擬機器上使用檢查點
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 166ef839a40452cc4156144e10e9c666e7ce3472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856149"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>請避免在生產環境中執行的伺服器工作負載的虛擬機器上使用檢查點

>適用於：Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  

在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。

> [!NOTE]  
> Windows Server 2012 r2 中虛擬機器快照已重新命名以符合 System Center Virtual Machine 管理中使用的術語的 HYPER-V 管理員中的虛擬機器檢查點。 如需詳細資訊，請參閱 < [Checkpoints and Snapshots Overview](https://technet.microsoft.com/library/dn818483.aspx)。  
  
## <a name="issue"></a>問題  
  
*已找到具有一或多個檢查點的虛擬機器。*  
  
## <a name="impact"></a>影響  
  
*會將檢查點檔案儲存在實體磁碟上可用的空間可能用盡。如果發生這種情況，就可以不執行任何額外的磁碟作業的實體儲存體上。可能會影響依賴的實體儲存體的任何虛擬機器。*  
  
如果實體磁碟空間執行，可能會自動暫停任何執行中虛擬機器檢查點或儲存在該磁碟上的虛擬硬碟。 HYPER-V 管理員中顯示這些虛擬機器的狀態為 暫停-重大 」。  
  
## <a name="resolution"></a>解析度  
  
*如果虛擬機器是在生產環境中執行伺服器工作負載，讓虛擬機器離線，然後再使用 套用或刪除檢查點的 HYPER-V 管理員。若要刪除檢查點，您必須關閉虛擬機器以完成程序。*  
  
> [!NOTE]  
> 生產檢查點現在可作為標準檢查點的替代方案。 如需詳細資訊，請參閱 <<c0> [ 標準或生產檢查點之間做選擇](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。  
  


