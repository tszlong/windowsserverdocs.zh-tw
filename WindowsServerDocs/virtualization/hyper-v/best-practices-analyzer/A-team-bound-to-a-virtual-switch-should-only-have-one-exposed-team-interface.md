---
title: 系結至虛擬交換器的小組應該只有一個公開的小組介面
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 413448945d2598ba36bed646144a43e39a1a3159
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857941"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>系結至虛擬交換器的小組應該只有一個公開的小組介面

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題
*一或多個虛擬交換器系結至具有多個小組介面的小組。*  
  
## <a name="impact"></a>影響
*下列虛擬交換器可能無法存取其他小組介面所使用的 Vlan 和頻寬：*  
  
\<虛擬交換器清單 >  
  
## <a name="resolution"></a>解析度
*使用 Windows PowerShell Cmdlet NetLbfoTeamNic，從預設的小組介面以外的小組移除所有小組介面。*  
  


