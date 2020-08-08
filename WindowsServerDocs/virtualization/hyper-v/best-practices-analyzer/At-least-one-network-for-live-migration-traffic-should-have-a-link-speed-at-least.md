---
title: 針對即時移轉流量，至少要有一個網路的連結速度至少必須為 1 Gbps
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 763b3ad598d91ee1ae63e6e53086a8ff06348f98
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960681"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>針對即時移轉流量，至少要有一個網路的連結速度至少必須為 1 Gbps

>適用於：Windows Server 2016



|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*即時移轉流量的網路都沒有至少 1 Gbps 的連結速度。*

## <a name="impact"></a>影響
*即時移轉的速度可能會變慢，這可能會因為 TCP 連接逾時而中斷網路連線。*

## <a name="resolution"></a>解決方法
*至少設定一個即時移轉網路，速度為 1 Gbps 或更快。*

請參閱網路硬體廠商的檔，以瞭解是否有任何現有的網路介面卡可支援至少 1 Gbps 的連結速度。



