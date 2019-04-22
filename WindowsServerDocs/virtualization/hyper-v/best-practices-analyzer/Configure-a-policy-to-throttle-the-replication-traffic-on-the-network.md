---
title: 設定原則以啟用節流設定在網路上的複寫流量
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2c1f1865fa1d611c0b5baaf981140f9807b51458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818689"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>設定原則以啟用節流設定在網路上的複寫流量

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*有可能不受限制的網路頻寬多寡，複寫便可以取用。*  
  
## <a name="impact"></a>影響  
*網路頻寬可能變成完全受到複寫流量，會影響其他重要的網路活動。這會影響下列連接埠：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*如果您使用另一種方法來進行節流處理網路流量時，您可以忽略此。若要設定將會進行節流處理的複本伺服器的相關通訊埠的網路流量的原則，否則使用群組原則編輯器。*  
  
  


