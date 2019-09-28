---
title: 所有用於即時移轉流量的網路都應具有至少 1 Gbps 的連結速度
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 92ba74ec75d8e90979e1cc329415a52af0218f54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365299"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>所有用於即時移轉流量的網路都應具有至少 1 Gbps 的連結速度

>適用於：Windows Server 2016


  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*即時移轉流量的網路都沒有至少 1 Gbps 的連結速度。*  
  
## <a name="impact"></a>影響  
*即時移轉的速度可能會變慢，這可能會因為 TCP 連接逾時而中斷網路連線。*  
  
## <a name="resolution"></a>解析度  
*至少設定一個即時移轉網路，速度為 1 Gbps 或更快。*  
  
請參閱網路硬體廠商的檔，以瞭解是否有任何現有的網路介面卡可支援至少 1 Gbps 的連結速度。  
  


