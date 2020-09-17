---
title: 系結至虛擬交換器的小組介面應該處於預設模式
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
ms.date: 8/16/2016
ms.openlocfilehash: 1e3d701ccc0ac7a6a3765df08f6b039fbb4ca3eb
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746133"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>系結至虛擬交換器的小組介面應該處於預設模式

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*某些虛擬交換器會系結至小組介面，但小組介面不會將所有 Vlan 的流量傳遞至虛擬交換器。*

## <a name="impact"></a>**影響**
*下列虛擬交換器無法存取所有 Vlan： \n{0}*

## <a name="resolution"></a>**解決方法**
*使用伺服器管理員或 Windows PowerShell Cmdlet NetLbfoTeamNic，將小組介面重設為預設模式。*



