---
title: 以足夠的動態 MAC 位址數量來設定伺服器
description: 瞭解可用的動態 MAC 位址數目很低時該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
ms.date: 8/16/2016
ms.openlocfilehash: d28035ab63f89986e78e5f17501f7df170742ad7
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846417"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>以足夠的動態 MAC 位址數量來設定伺服器

>適用於：Windows Server 2016

*本主題旨在解決最佳做法分析程式掃描所識別的特定問題。本主題中的資訊僅適用于已對其執行 Hyper-v 最佳做法分析程式，且遇到本主題所解決之問題的電腦。如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*可用的動態 MAC 位址數目很低。*

## <a name="impact"></a>影響

*如果沒有可用的動態 MAC 位址，則無法啟動設定為使用動態 MAC 位址的虛擬機器。*

## <a name="resolution"></a>解決方案

*使用虛擬交換器管理員來查看和延伸動態位址的範圍。*



