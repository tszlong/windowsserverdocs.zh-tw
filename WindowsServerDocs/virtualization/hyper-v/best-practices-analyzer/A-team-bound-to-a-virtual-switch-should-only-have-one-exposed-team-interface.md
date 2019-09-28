---
title: 系結至虛擬交換器的小組應該只有一個公開的小組介面
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6baa9e4ae900c9b671003872b4eb4589efb2f085
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365396"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>系結至虛擬交換器的小組應該只有一個公開的小組介面

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多個虛擬交換器系結至具有多個小組介面的小組。*  
  
## <a name="impact"></a>**產生**  
*下列虛擬交換器可能無法存取其他小組介面所使用的 Vlan 和頻寬：*  
  
@no__t 虛擬交換器的 0list >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Windows PowerShell Cmdlet NetLbfoTeamNic，從預設的小組介面以外的小組移除所有小組介面。*  
  


