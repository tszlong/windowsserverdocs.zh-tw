---
title: 使用足夠數量的動態 MAC 位址來設定伺服器
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1fe7b34abc8be9777a491a08016627e7b2a66a2a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862041"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>使用足夠數量的動態 MAC 位址來設定伺服器

>適用於︰Windows Server 2016

*本主題旨在解決最佳做法分析程式掃描所識別的特定問題。本主題中的資訊僅適用于已執行 Hyper-v 最佳做法分析程式的電腦，並遇到本主題所解決的問題。如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*可用的動態 MAC 位址數目很低。*  
  
## <a name="impact"></a>影響  
  
*如果沒有可用的動態 MAC 位址，則無法啟動設定為使用動態 MAC 位址的虛擬機器。*  
  
## <a name="resolution"></a>解析度  
  
*使用虛擬交換器管理員來查看和擴充動態位址的範圍。*  
  


