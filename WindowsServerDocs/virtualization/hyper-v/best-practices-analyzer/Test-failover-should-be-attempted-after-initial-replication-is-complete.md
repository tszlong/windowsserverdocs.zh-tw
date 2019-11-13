---
title: 初始複寫完成後，應該嘗試測試容錯移轉
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec3ad3994227eb14d1d2e53842c755af76ac538d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364696"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>初始複寫完成後，應該嘗試測試容錯移轉

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|操作|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="problem"></a>問題  
*至少有一個月內沒有測試容錯移轉。*  
  
## <a name="impact"></a>影響  
*不會確認已規劃或未計畫的容錯移轉將會成功，或工作負載作業會在容錯移轉後繼續正常運作。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*使用 Hyper-v 管理員來進行測試容錯移轉。*  
  


