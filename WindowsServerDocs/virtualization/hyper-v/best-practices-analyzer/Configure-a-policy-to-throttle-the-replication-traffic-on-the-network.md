---
title: 設定原則以節流網絡上的複寫流量
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2c358cd930f2b95412b40aa6c87b0bf9ebb5b741
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862171"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>設定原則以節流網絡上的複寫流量

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*允許複寫使用的網路頻寬量可能不會有限制。*  
  
## <a name="impact"></a>影響  
*網路頻寬可能會因為複寫流量而受到全面的影響，因而影響其他重要的網路活動。這會影響下列埠：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*如果您使用另一種方法來節流處理網路流量，您可以忽略此情況。否則，請使用群組原則編輯器來設定原則，以將網路流量節流到複本伺服器的相關埠。*  
  
  


