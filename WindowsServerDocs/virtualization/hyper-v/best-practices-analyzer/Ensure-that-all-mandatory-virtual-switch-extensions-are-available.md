---
title: 確定所有必要的虛擬交換器擴充功能都可供使用
description: 瞭解當一或多個虛擬網路介面卡連接到具有已停用或未安裝之強制擴充功能的虛擬交換器時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
ms.date: 8/16/2016
ms.openlocfilehash: 0af9d5823740ebcfaaf740f8738ea91d4d827baa
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846132"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>確定所有必要的虛擬交換器擴充功能都可供使用

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*有一或多個虛擬網路介面卡連接到具有已停用或未安裝之強制擴充功能的虛擬交換器。*

## <a name="impact"></a>影響
*下列虛擬機器的一或多個虛擬網路介面卡上的網路流量遭到封鎖：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*首先，請確定已在主機上安裝必要的擴充功能，並視需要安裝延伸模組。然後，如果已停用強制擴充功能，請使用虛擬交換器管理員或 Windows PowerShell Cmdlet Enable-VMSwitchExtension 來啟用擴充功能。*



