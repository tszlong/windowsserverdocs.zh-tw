---
title: 必須在與外部虛擬交換器系結的 VMQ 功能實體網路介面卡上啟用 VMQ
description: 瞭解當下列網路介面卡能夠 (VMQ) 的虛擬機器佇列，但功能已停用時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
ms.date: 8/16/2016
ms.openlocfilehash: 1ab79c106fd17d459a1b6b6adcedba620ae5a89c
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846207"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>必須在與外部虛擬交換器系結的 VMQ 功能實體網路介面卡上啟用 VMQ

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
*下列網路介面卡可 (VMQ) 的虛擬機器佇列，但已停用此功能。*

## <a name="impact"></a>**影響**
*Windows 無法充分利用下列網路介面卡上的可用硬體卸載：*

\<list of network adapters>

## <a name="resolution"></a>**解決方法**
*使用 Enable-NetAdapterVmq Windows PowerShell Cmdlet 或使用網路介面卡的 Advanced Properties 使用者介面來啟用 VMQ。*



