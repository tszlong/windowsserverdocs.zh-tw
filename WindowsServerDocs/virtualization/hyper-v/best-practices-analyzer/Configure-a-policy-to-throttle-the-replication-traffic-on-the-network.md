---
title: 設定原則以節流網絡上的複寫流量
description: 瞭解當複寫允許使用的網路頻寬數量可能沒有限制時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
ms.date: 8/16/2016
ms.openlocfilehash: 69a03e85dd410f1f3b59daaf97ff97c2db5c7d48
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834843"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>設定原則以節流網絡上的複寫流量

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
*允許複寫使用的網路頻寬數量可能不會有限制。*

## <a name="impact"></a>影響
*網路頻寬可能會完全由複寫流量支配，因而影響其他重要的網路活動。這會影響下列埠：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*如果您使用另一種方法來節流處理網路流量，您可以忽略此情況。否則，請使用群組原則編輯器來設定原則，以將網路流量節流至複本伺服器的相關埠。*




