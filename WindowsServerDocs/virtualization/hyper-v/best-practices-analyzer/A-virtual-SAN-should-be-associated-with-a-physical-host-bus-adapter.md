---
title: 應該與實體主機匯流排介面卡相關聯的虛擬 SAN
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3b9ca1e2da1cf9f4410f465fe95c6cc9c0b07ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819079"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>應該與實體主機匯流排介面卡相關聯的虛擬 SAN

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*已設定虛擬存放區域網路 (SAN) 沒有關聯的主機匯流排介面卡 (HBA)。*  
  
## <a name="impact"></a>**影響**  
*虛擬機器將無法啟動時設定虛擬光纖通道介面卡連線到設定錯誤的虛擬 SAN。這會影響下列虛擬 San:*  
  
  
\<虛擬 San 的清單 >  
  
  
## <a name="resolution"></a>**解決方法**  
*重新連接到主機匯流排介面卡設定的虛擬 SAN。*  
  
  
  


