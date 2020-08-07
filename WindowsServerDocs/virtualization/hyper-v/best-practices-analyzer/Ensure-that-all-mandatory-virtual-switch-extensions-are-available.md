---
title: 確定所有強制的虛擬交換器擴充功能都可供使用
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 48c8ee80a0044c067d29730c1ec79bd67fdc062b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950252"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>確定所有強制的虛擬交換器擴充功能都可供使用

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*一或多個虛擬網路介面卡連線至虛擬交換器，且其強制擴充功能已停用或未安裝。*

## <a name="impact"></a>影響
*下列虛擬機器上的一或多個虛擬網路介面卡上的網路流量遭到封鎖：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*首先，請確定已在主機上安裝必要的延伸模組，並視需要安裝擴充功能。然後，如果強制的延伸模組已停用，請使用虛擬交換器管理員或 Windows PowerShell Cmdlet VMSwitchExtension 來啟用擴充功能。*



