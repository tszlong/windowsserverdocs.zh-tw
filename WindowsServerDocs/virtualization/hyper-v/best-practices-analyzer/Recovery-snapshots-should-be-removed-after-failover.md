---
title: 在容錯移轉之後，應該移除復原快照集
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4663320df91019fc7dc1d8ca7ffdb2fcc3e0de42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837679"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>在容錯移轉之後，應該移除復原快照集

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016| 
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*容錯移轉虛擬機器有一或多個復原快照集。*  
  
## <a name="impact"></a>**影響**  
*可用的空間可能會用盡儲存快照集檔案的實體磁碟上。如果發生這種情況，就可以不執行任何額外的磁碟作業的實體儲存體上。可能會影響依賴的實體儲存體的任何虛擬機器。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*針對每個容錯移轉虛擬機器，在 Windows PowerShell 中使用 Complete-vmfailover cmdlet 移除復原快照集，並指出容錯移轉完成。*  
  


