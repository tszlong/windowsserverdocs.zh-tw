---
title: 小組介面繫結至虛擬交換器應該是在預設模式
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 22e5ad0eed6e6ea07a83150762b76163442f2c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872159"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>小組介面繫結至虛擬交換器應該是在預設模式

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
*一些虛擬交換器繫結至小組介面，但小組介面不會將流量傳遞所有的 Vlan 上為虛擬交換器。*  
  
## <a name="impact"></a>**影響**  
*下列的虛擬交換器不能有存取所有的 Vlan: \n{0}*  
  
## <a name="resolution"></a>**解決方法**  
*使用伺服器管理員或 Windows PowerShell cmdlet 設定 NetLbfoTeamNic 重小組介面設為預設模式。*  
  


