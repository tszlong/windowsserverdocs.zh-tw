---
title: 必須在系結至外部虛擬交換器的具備 VMQ 功能的實體網路介面卡上啟用 VMQ
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ae8c5005f9038ac2b82fd3f56016da357a4a9364
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960231"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>必須在系結至外部虛擬交換器的具備 VMQ 功能的實體網路介面卡上啟用 VMQ

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*下列網路介面卡可以 (VMQ) 的虛擬機器佇列，但此功能已停用。*

## <a name="impact"></a>**影響**
*Windows 無法充分利用下列網路介面卡上可用的硬體卸載：*

\<list of network adapters>

## <a name="resolution"></a>**解決方案**
*使用 Get-netadaptervmq Windows PowerShell Cmdlet 或使用網路介面卡的 Advanced Properties 使用者介面來啟用 VMQ。*



