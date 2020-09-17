---
title: 應至少每個月執行一次測試容錯移轉，以確認容錯移轉會成功，且虛擬機器工作負載將在容錯移轉之後以預期的方式運作
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
ms.date: 8/16/2016
ms.openlocfilehash: 4ec97d55f1a6caf33b1b46d0a6bdb4e99005c971
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90744091"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>應至少每個月執行一次測試容錯移轉，以確認容錯移轉會成功，且虛擬機器工作負載將在容錯移轉之後以預期的方式運作

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*至少有一個月的測試容錯移轉。*

## <a name="impact"></a>影響
*未確認規劃或未規劃的容錯移轉會成功，或工作負載作業將在容錯移轉之後繼續正常運作。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*使用 Hyper-v 管理員進行測試容錯移轉。*



