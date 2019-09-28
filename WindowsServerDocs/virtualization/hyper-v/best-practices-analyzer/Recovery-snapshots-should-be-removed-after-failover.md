---
title: 容錯移轉後應移除復原快照集
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4b8574956fb1b46ca0cf9678187fffcd68c2d261
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393534"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>容錯移轉後應移除復原快照集

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016| 
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|作業|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*已故障的虛擬機器有一或多個復原快照集。*  
  
## <a name="impact"></a>**產生**  
在儲存快照集檔案的實體磁片上，@no__t 0Available 空間可能會用盡。如果發生這種情況，就無法在實體儲存體上執行額外的磁片作業。依賴實體存放裝置的任何虛擬機器都可能受到影響。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>**解決方法**  
*針對每個已容錯移轉的虛擬機器，在 Windows PowerShell 中使用 Start-vmfailover 指令程式來移除復原快照，並指出容錯移轉完成。*  
  


