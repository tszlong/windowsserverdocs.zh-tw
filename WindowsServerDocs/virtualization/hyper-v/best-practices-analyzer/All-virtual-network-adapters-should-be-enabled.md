---
title: 應啟用所有虛擬網路介面卡
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
ms.date: 8/16/2016
ms.openlocfilehash: 48417108f780c8b145613b110bb51681bd69bdc6
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746303"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>應啟用所有虛擬網路介面卡

>適用於：Windows Server 2016



|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*管理作業系統中已停用一或多個與實體網路介面卡相關聯的虛擬網路介面卡。*

## <a name="impact"></a>影響

*此伺服器的設定不是最佳的設定。*

管理作業系統無法使用這部電腦上的其中一張實體網路介面卡連接到實體 (外部) 網路，因為它與已停用的虛擬網路介面卡相關聯。

## <a name="resolution"></a>解決方案

*使用網路 & 網際網路設定來啟用虛擬網路介面卡。或者，使用虛擬交換器管理員重新設定外部虛擬交換器，使其不會與管理作業系統共用。*



