---
title: 必須在與外部虛擬交換器系結的 VMQ 功能實體網路介面卡上啟用 VMQ
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
ms.date: 8/16/2016
ms.openlocfilehash: 169f2ea06bf35c7bbc9bcaec354ca66e118c75d2
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746763"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>必須在與外部虛擬交換器系結的 VMQ 功能實體網路介面卡上啟用 VMQ

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
*下列網路介面卡可 (VMQ) 的虛擬機器佇列，但已停用此功能。*

## <a name="impact"></a>**影響**
*Windows 無法充分利用下列網路介面卡上的可用硬體卸載：*

\<list of network adapters>

## <a name="resolution"></a>**解決方法**
*使用 NetAdapterVmq Windows PowerShell Cmdlet 或使用網路介面卡的 Advanced Properties 使用者介面來啟用 VMQ。*



