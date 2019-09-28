---
title: 若要參與複寫，容錯移轉叢集中的伺服器必須已設定 Hyper-v 複本代理人
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2e15d2c4a467807397ef4712d2df1730b40d8024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364603"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>若要參與複寫，容錯移轉叢集中的伺服器必須已設定 Hyper-v 複本代理人

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|Error|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*針對容錯移轉叢集，Hyper-v 複本需要使用 Hyper-v 複本代理人名稱，而不是個別的伺服器名稱。*  
  
## <a name="impact"></a>影響  
*如果虛擬機器已移至不同的容錯移轉叢集節點，則無法繼續複寫。*  
  
## <a name="resolution"></a>解析度  
@no__t 0Use 容錯移轉叢集管理員以設定 Hyper-v 複本代理人。在 [Hyper-v 管理員] 中，確定複寫設定使用 Hyper-v 複本代理人名稱做為伺服器名稱。 *  
  


