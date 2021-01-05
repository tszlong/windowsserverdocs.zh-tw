---
title: 系結至虛擬交換器的小組介面應該處於預設模式
description: 瞭解當某些虛擬交換器系結至小組介面時該怎麼辦，但是小組介面不會將所有 Vlan 的流量傳遞至虛擬交換器。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
ms.date: 8/16/2016
ms.openlocfilehash: e12ed7286a3aba4d6bb4f4bd3c72481326f44e0f
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845897"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>系結至虛擬交換器的小組介面應該處於預設模式

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*某些虛擬交換器會系結至小組介面，但小組介面不會將所有 Vlan 的流量傳遞至虛擬交換器。*

## <a name="impact"></a>**影響**
*下列虛擬交換器無法存取所有 Vlan： \n{0}*

## <a name="resolution"></a>**解決方法**
*使用伺服器管理員或 Windows PowerShell Cmdlet Set-NetLbfoTeamNic，將小組介面重設為預設模式。*



