---
title: 離峰時間應該排定的重新同步處理的複寫
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2d6c18b7e37c5d17f56f41c7ff03ed8796457de0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840019"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>離峰時間應該排定的重新同步處理的複寫

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*重新同步處理的主要虛擬機器的複寫不會排定離峰時間。*  
  
## <a name="impact"></a>影響  
*較長的虛擬機器處於需要重新同步處理的狀態、 更長的複寫記錄檔成長並更未複寫的變更發生在主要虛擬機器上。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*您可以使用 HYPER-V 管理員來修改虛擬機器的複寫設定，執行重新同步處理會自動在離峰時間。*  
  


