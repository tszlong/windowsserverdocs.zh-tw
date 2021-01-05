---
title: 系結至虛擬交換器的小組應該只有一個公開的小組介面
description: 瞭解當一或多個虛擬交換器系結至具有多個小組介面的小組時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
ms.date: 8/16/2016
ms.openlocfilehash: af7b7b80559004c765a4a964b0e72827d74136e7
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834763"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>系結至虛擬交換器的小組應該只有一個公開的小組介面

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*一或多個虛擬交換器系結至具有多個小組介面的小組。*

## <a name="impact"></a>影響
*下列虛擬交換器可能無法存取其他小組介面所使用的 Vlan 和頻寬：*

\<list of virtual switches>

## <a name="resolution"></a>解決方案
*使用 Windows PowerShell Cmdlet Remove-NetLbfoTeamNic 移除小組的所有小組介面，而非預設 team 介面。*



