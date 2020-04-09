---
title: 虛擬 SAN 應該與實體主機匯流排介面卡相關聯
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 10a17f8661f1436f5b4db317648edde57a0dfe74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857931"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>虛擬 SAN 應該與實體主機匯流排介面卡相關聯

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*已設定虛擬存放區域網路（SAN），而沒有與主機匯流排介面卡（HBA）的關聯。*  
  
## <a name="impact"></a>**產生**  
*當虛擬機器使用連線至設定錯誤之虛擬 SAN 的虛擬光纖通道介面卡時，將無法啟動。這會影響下列虛擬 San：*  
  
  
\<虛擬 San 清單 >  
  
  
## <a name="resolution"></a>**解決方法**  
*將虛擬 SAN 連接到主機匯流排介面卡，以重新設定它。*  
  
  
  


