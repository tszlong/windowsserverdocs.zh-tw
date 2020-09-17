---
title: 所有適用于即時移轉流量的網路，其連結速度至少應為 1 Gbps
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
ms.date: 8/16/2016
ms.openlocfilehash: e73f17a790ac64942ea1ca608d4eeaa9b18402de
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746313"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>所有適用于即時移轉流量的網路，其連結速度至少應為 1 Gbps

> 適用於：Windows Server 2016

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*即時移轉流量沒有任何網路的連結速度至少為 1 Gbps。*

## <a name="impact"></a>影響
*即時移轉可能會變慢，因為 TCP 連接逾時，可能會中斷網路連接。*

## <a name="resolution"></a>解決方案
*至少以 1 Gbps 或更快的速度設定一個即時移轉網路。*

請參閱網路硬體廠商的檔，以瞭解是否有任何現有的網路介面卡可支援至少 1 Gbps 的連結速度。



