---
title: 系結至虛擬交換器的小組介面應處於預設模式
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: fe19de5dd380d08c01c917da9d4e2ef9465de042
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854601"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>系結至虛擬交換器的小組介面應處於預設模式

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*某些虛擬交換器會系結至小組介面，但小組介面不會將所有 Vlan 的流量傳遞至虛擬交換器。*  
  
## <a name="impact"></a>**產生**  
*下列虛擬交換器無法存取所有 Vlan： \n{0}*  
  
## <a name="resolution"></a>**解決方法**  
*使用伺服器管理員或 Windows PowerShell Cmdlet NetLbfoTeamNic，將小組介面重設為預設模式。*  
  


