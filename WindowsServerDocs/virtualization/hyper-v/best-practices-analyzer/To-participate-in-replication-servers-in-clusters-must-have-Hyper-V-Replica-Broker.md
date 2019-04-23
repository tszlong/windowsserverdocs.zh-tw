---
title: 若要參與複寫，在容錯移轉叢集中的伺服器必須設定 HYPER-V 複本代理人
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d4966396af955f9c8bad34b5b2892115e93c3b85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887969"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>若要參與複寫，在容錯移轉叢集中的伺服器必須設定 HYPER-V 複本代理人

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*HYPER-V 複本容錯移轉叢集，需要使用 HYPER-V 複本代理人的名稱，而不是個別的伺服器名稱。*  
  
## <a name="impact"></a>影響  
*如果虛擬機器移到不同的容錯移轉叢集節點中，無法繼續複寫。*  
  
## <a name="resolution"></a>解析度  
*使用容錯移轉叢集管理員來設定 HYPER-V 複本代理人。在 [HYPER-V 管理員] 中，請確定複寫組態會使用 HYPER-V 複本代理人的名稱，做為伺服器名稱。*  
  


