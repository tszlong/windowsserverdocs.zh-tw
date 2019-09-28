---
title: 使用中的邏輯處理器數目不得超過支援的最大值
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 380daf333c041c8702228a60c26ab6e76e4cf3e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393402"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>使用中的邏輯處理器數目不得超過支援的最大值

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|Error|  
|**分類**|原則|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的文字。  
  
## <a name="issue"></a>問題  
  
*伺服器設定了太多邏輯處理器。*  
  
## <a name="impact"></a>影響  
  
*Microsoft 不支援在這部電腦上執行 Hyper-v。*  
  
## <a name="resolution"></a>解析度  
  
*請從這部電腦移除一些處理器，或使用 msconfig 來限制可用的處理器數目。*  
  
請參閱下列指示以使用 Msconfig。 如需移除處理器的詳細資訊，請參閱電腦隨附的指示，或洽詢硬體製造商。 如需 Hyper-v 支援的最大設定的詳細資訊，請參閱[在 Windows Server 2016 中規劃 hyper-v 擴充性](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)。  
  
### <a name="to-limit-the-number-of-available-processors"></a>限制可用的處理器數目  
  
1.  開啟 [系統組態] 應用程式（Msconfig.exe）。 若要這麼做，請按一下 [**開始**]，輸入**msconfig**，以滑鼠右鍵按一下**系統**設定桌面應用程式，然後按一下 [**以系統管理員身分執行**]。  
  
2.  在 [**開機**] 索引標籤上，按一下 [ **Advanced options**]。  
  
3.  選取 [**處理器數目**]，然後在清單中選取一個數位。 按一下 [確定]。  
  
4.  重新開機電腦，以使用新的處理器數目來執行。  
  


