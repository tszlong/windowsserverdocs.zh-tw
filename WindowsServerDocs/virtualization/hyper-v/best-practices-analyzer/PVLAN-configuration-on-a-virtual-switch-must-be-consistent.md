---
title: 虛擬交換器上的 PVLAN 設定必須一致
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 63780266fa23187e12bfd5d503a1f3c0306bc364
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939025"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>虛擬交換器上的 PVLAN 設定必須一致

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*未在一或多個虛擬網路介面卡上正確設定私人虛擬區域網路絡 (PVLAN) 。*

## <a name="impact"></a>**影響**
*PVLAN 可能不會正確地隔離虛擬機器之間的網路流量。*

## <a name="resolution"></a>**解決方案**
*使用 Windows PowerShell Cmdlet Set-vmnetworkadaptervlan，正確設定 PVLAN。*



