---
title: 繫結至虛擬交換器的小組應該只有一個公開的小組介面
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 108bbec1439959bb7ab4475b59c7231653952ea8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838459"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>繫結至虛擬交換器的小組應該只有一個公開的小組介面

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多個虛擬交換器繫結至具有多個小組介面的小組。*  
  
## <a name="impact"></a>**影響**  
*下列的虛擬交換器可能沒有存取 Vlan 與其他小組介面所使用的頻寬：*  
  
\<虛擬交換器的清單 >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Windows PowerShell cmdlet 移除 NetLbfoTeamNic 移除預設小組介面以外，小組的小組的所有介面。*  
  


