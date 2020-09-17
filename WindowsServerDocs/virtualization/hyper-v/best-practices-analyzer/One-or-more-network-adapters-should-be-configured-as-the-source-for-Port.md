---
title: 應將一或多個網路介面卡設定為埠鏡像的來源
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
ms.date: 8/16/2016
ms.openlocfilehash: c8a04001986ab4d3722c9e51a5d5ceb7cbd0bf02
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746243"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>應將一或多個網路介面卡設定為埠鏡像的來源

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
*一或多部虛擬機器的網路介面卡設定為埠鏡像的目的地，但虛擬交換器上沒有對應的來源。*

## <a name="impact"></a>**影響**
*下列虛擬交換器和虛擬機器的埠鏡像將無法正確運作：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*使用 Windows PowerShell 或 Hyper-v 管理員來完成或更正埠鏡像設定。*



