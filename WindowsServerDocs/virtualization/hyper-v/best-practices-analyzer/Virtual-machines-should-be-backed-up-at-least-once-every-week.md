---
title: 虛擬機器都應該備份至少一次每週
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e079e3cb225ec9c712233bbf3efc85bb6f09b218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826749"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>虛擬機器都應該備份至少一次每週

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*一或多個虛擬機器有尚未備份過去一週。*  
  
## <a name="impact"></a>影響  
*如果虛擬機器發生問題時，新的備份不存在，則可能會發生重大資料遺失。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*排程執行至少一次一週的虛擬機器備份。您可以忽略這個規則如果這部虛擬機器是複本，且其主要的虛擬機器備份，或者如果這是主要的虛擬機器，且其複本正在進行備份。*  
  


