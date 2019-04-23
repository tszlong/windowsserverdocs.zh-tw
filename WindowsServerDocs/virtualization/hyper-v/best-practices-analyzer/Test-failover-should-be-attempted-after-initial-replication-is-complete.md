---
title: 初始複寫完成之後，就應該嘗試測試容錯移轉
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 347b089330548e5fb2631e43ca66c96bb6bc4e9d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880409"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>初始複寫完成之後，就應該嘗試測試容錯移轉

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="problem"></a>問題  
*在至少一個月內已經過任何測試容錯移轉。*  
  
## <a name="impact"></a>影響  
*沒有任何計劃性或非計劃性容錯移轉會成功，或在容錯移轉之後的 「 工作負載作業會繼續進行正常的確認。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*您可以使用 HYPER-V 管理員來進行測試容錯移轉。*  
  


