---
title: 初始複寫完成後，應嘗試測試容錯移轉
description: 瞭解在初始複寫完成之後，沒有測試容錯移轉時該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
ms.date: 8/16/2016
ms.openlocfilehash: 5f97d6c5d9a44691af22506008dd20cd6f7a45a4
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845856"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>初始複寫完成後，應嘗試測試容錯移轉

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="problem"></a>問題
*至少有一個月的測試容錯移轉。*

## <a name="impact"></a>影響
*未確認規劃或未規劃的容錯移轉會成功，或工作負載作業將在容錯移轉之後繼續正常運作。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*使用 Hyper-v 管理員進行測試容錯移轉。*



