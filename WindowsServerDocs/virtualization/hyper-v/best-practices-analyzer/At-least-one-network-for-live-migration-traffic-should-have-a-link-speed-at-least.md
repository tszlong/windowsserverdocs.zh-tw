---
title: 即時移轉流量的至少一個網路應該有至少 1 Gbps 的連結速度
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ef75f73bd934b863b146e93f4cdc7323e5d4c6fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837729"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>即時移轉流量的至少一個網路應該有至少 1 Gbps 的連結速度

>適用於：Windows Server 2016


  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*即時移轉流量的網路都至少 1 Gbps 的連結速度。*  
  
## <a name="impact"></a>影響  
*即時移轉可能會發生速度緩慢，這可能會中斷網路連線，因為在 TCP 連線逾時。*  
  
## <a name="resolution"></a>解析度  
*至少一個即時移轉網路設定的 1 Gbps 或更快的速度。*  
  
請參閱您若要了解是否任何現有的網路介面卡可以支援連結速度至少 1 Gbps 的網路硬體廠商的文件。  
  


