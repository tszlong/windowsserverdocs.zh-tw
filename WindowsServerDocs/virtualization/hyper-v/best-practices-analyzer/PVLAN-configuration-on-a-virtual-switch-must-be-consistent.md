---
title: 虛擬交換器上的 PVLAN 設定必須一致
description: 瞭解在一或多個虛擬網路介面卡上未正確設定私人虛擬區域網路絡 (PVLAN) 時該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
ms.date: 8/16/2016
ms.openlocfilehash: d09f1d06c691563ecac9d4ed29fee977588443de
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846342"
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

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*未在一或多個虛擬網路介面卡上正確設定私人虛擬區域網路絡 (PVLAN) 。*

## <a name="impact"></a>**影響**
*PVLAN 可能無法正確隔離虛擬機器之間的網路流量。*

## <a name="resolution"></a>**解決方法**
*使用 Windows PowerShell Cmdlet Get-vmnetworkadaptervlan，以正確設定 PVLAN。*



