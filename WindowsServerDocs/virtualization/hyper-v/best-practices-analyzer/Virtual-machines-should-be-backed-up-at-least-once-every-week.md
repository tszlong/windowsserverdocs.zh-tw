---
title: 虛擬機器至少應備份每週一次
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6cb425f92926aa1823ed89cd26afccc2d962603d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855031"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>虛擬機器至少應備份每週一次

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|錯誤|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*一部或多部虛擬機器在過去一周內尚未備份。*  
  
## <a name="impact"></a>影響  
*如果虛擬機器遇到問題，而且最近的備份不存在，可能會發生重大資料遺失的情況。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*將虛擬機器的備份排程為每週至少執行一次。如果此虛擬機器是複本，而且其主要虛擬機器正在進行備份，或如果這是主要虛擬機器，且其複本正在進行備份，您可以忽略此規則。*  
  


