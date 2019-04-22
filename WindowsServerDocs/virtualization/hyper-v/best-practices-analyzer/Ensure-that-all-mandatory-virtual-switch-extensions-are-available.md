---
title: 確定所有必要的虛擬交換器擴充功能可供使用
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 53ceeb9aab6ca7196454fbcd7f0fdae8b34d05d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825939"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>確定所有必要的虛擬交換器擴充功能可供使用

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
*一或多個虛擬網路介面卡會連接到已停用或未安裝的必要項目擴充功能的虛擬交換器。*  
  
## <a name="impact"></a>影響  
*在下列虛擬機器上的一或多個虛擬網路介面卡上封鎖網路流量：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*首先，請確定主機已安裝必要的延伸模組，並視需要安裝擴充功能。然後，如果必要的延伸模組已停用，使用虛擬交換器管理員] 或 [Windows PowerShell cmdlet Enable-vmswitchextension 啟用該擴充功能。*  
  


