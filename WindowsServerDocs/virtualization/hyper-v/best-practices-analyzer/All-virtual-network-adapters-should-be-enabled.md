---
title: 應啟用所有虛擬網路介面卡
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 286a0dc099eb6350fe7f5adef925a2d38d99a3ca
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946094"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>應啟用所有虛擬網路介面卡

>適用於：Windows Server 2016



|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*與實體網路介面卡相關聯的一或多個虛擬網路介面卡，在管理作業系統中已停用。*

## <a name="impact"></a>影響

*此伺服器的設定不是最佳設定。*

管理作業系統無法使用這部電腦上的其中一個實體網路介面卡連線到實體 (外部) 網路，因為它與已停用的虛擬網路介面卡相關聯。

## <a name="resolution"></a>解決方法

*使用網路 & 網際網路設定來啟用虛擬網路介面卡。或者，使用虛擬交換器管理員重新設定外部虛擬交換器，使其不會與管理作業系統共用。*



