---
title: 虛擬機器至少應備份每週一次
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec11b067de2c9f8cbb3a17731caa0dc526bf54a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393233"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>虛擬機器至少應備份每週一次

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|Error|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*一部或多部虛擬機器在過去一周內尚未備份。*  
  
## <a name="impact"></a>影響  
如果虛擬機器遇到問題，而且最近的備份不存在，可能會發生 @no__t 0Significant 的資料遺失。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>解析度  
@no__t 0Schedule 虛擬機器的備份，一周至少執行一次。如果此虛擬機器是複本，而且其主要虛擬機器正在進行備份，或如果這是主要虛擬機器，且其複本正在進行備份，您可以忽略此規則。 *  
  


