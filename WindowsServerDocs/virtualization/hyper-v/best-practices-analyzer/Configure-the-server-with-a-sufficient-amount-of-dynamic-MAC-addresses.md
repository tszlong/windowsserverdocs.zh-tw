---
title: 設定伺服器具有足夠容量的動態 MAC 位址
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fc444225c38ef7e8605ec328cfe3f8184b2fd307
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870729"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>設定伺服器具有足夠容量的動態 MAC 位址

>適用於：Windows Server 2016

*本主題旨在解決最佳做法分析程式掃描所識別的特定問題。您應該已經 HYPER-V Best Practices Analyzer 對它們執行，且發生本主題提及之問題的電腦套用本主題中的資訊。如需最佳做法和掃描的詳細資訊，請參閱* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*可用的動態 MAC 位址的數目很低。*  
  
## <a name="impact"></a>影響  
  
*沒有動態 MAC 位址可用時，就無法啟動虛擬機器設定為使用動態 MAC 位址。*  
  
## <a name="resolution"></a>解析度  
  
*使用虛擬交換器管理員來檢視和擴充的動態位址範圍。*  
  


