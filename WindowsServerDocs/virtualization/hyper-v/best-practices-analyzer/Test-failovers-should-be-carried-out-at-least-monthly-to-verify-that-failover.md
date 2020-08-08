---
title: 測試容錯移轉應至少每月執行一次，以確認容錯移轉會成功，且虛擬機器工作負載會在容錯移轉後如預期般運作
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: e758dbc98ff9bcb554fec8263704d788040b563f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960551"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>測試容錯移轉應至少每月執行一次，以確認容錯移轉會成功，且虛擬機器工作負載會在容錯移轉後如預期般運作

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*至少有一個月內沒有測試容錯移轉。*

## <a name="impact"></a>影響
*不會確認已規劃或未計畫的容錯移轉將會成功，或工作負載作業會在容錯移轉後繼續正常運作。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*使用 Hyper-v 管理員來進行測試容錯移轉。*



