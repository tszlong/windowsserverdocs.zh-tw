---
title: 使用中的邏輯處理器的數目不得超過支援的最大值
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d1275a17cc04494708f5ecfe9b708834b4233641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847069"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>使用中的邏輯處理器的數目不得超過支援的最大值

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|原則|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的文字。  
  
## <a name="issue"></a>問題  
  
*伺服器已使用太多邏輯處理器。*  
  
## <a name="impact"></a>影響  
  
*Microsoft 不支援此電腦上執行 HYPER-V。*  
  
## <a name="resolution"></a>解析度  
  
*從這台電腦中移除某些處理器，或使用 msconfig 限制可用的處理器數目。*  
  
請參閱下列指示來使用 Msconfig。 如需移除處理器的詳細資訊，請參閱電腦隨附的指示，或連絡硬體製造商做區分。 如需 HYPER-V 支援的最大組態的詳細資訊，請參閱[規劃 Windows Server 2016 中的 HYPER-V 延展性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
### <a name="to-limit-the-number-of-available-processors"></a>若要限制可用的處理器數目  
  
1.  開啟 [系統] 設定應用程式 (Msconfig.exe)。 若要這樣做，請按一下**開始**，型別**msconfig**，以滑鼠右鍵按一下**系統組態**傳統型應用程式，然後按一下**系統管理員身分執行**。  
  
2.  從**開機**索引標籤上，按一下**進階選項**。  
  
3.  選取 **處理器數目**，然後選取清單中的 數字。 按一下 [確定] 。  
  
4.  重新啟動電腦以執行使用新的處理器數目。  
  


