---
title: 應啟用所有虛擬網路介面卡
description: 瞭解當管理作業系統中已停用與實體網路介面卡相關聯的一或多個虛擬網路介面卡時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
ms.date: 8/16/2016
ms.openlocfilehash: 6b3fcd786a1f7f294b7c35a4dc0a8b3cf0971c83
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834753"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>應啟用所有虛擬網路介面卡

>適用於：Windows Server 2016



|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*管理作業系統中已停用一或多個與實體網路介面卡相關聯的虛擬網路介面卡。*

## <a name="impact"></a>影響

*此伺服器的設定不是最佳的設定。*

管理作業系統無法使用這部電腦上的其中一張實體網路介面卡連接到實體 (外部) 網路，因為它與已停用的虛擬網路介面卡相關聯。

## <a name="resolution"></a>解決方案

*使用網路 & 網際網路設定來啟用虛擬網路介面卡。或者，使用虛擬交換器管理員重新設定外部虛擬交換器，使其不會與管理作業系統共用。*



