---
title: 一或多個網路介面卡應設定為埠鏡像的來源
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1d5f355e4553d5d775152ed405729fa32c829acf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954455"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>一或多個網路介面卡應設定為埠鏡像的來源

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*一部或多部虛擬機器的網路介面卡設定為埠鏡像的目的地，但虛擬交換器上沒有對應的來源。*

## <a name="impact"></a>**影響**
*下列虛擬交換器和虛擬機器的埠鏡像將無法正確運作：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方案**
*使用 Windows PowerShell 或 Hyper-v 管理員來完成或更正埠鏡像設定。*



