---
title: 使用中的邏輯處理器數目不得超過支援的上限
description: 瞭解當伺服器設定的邏輯處理器太多時該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
ms.date: 8/16/2016
ms.openlocfilehash: 469f1a816be5b9ebd714a1c52b279111fed46da0
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833803"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>使用中的邏輯處理器數目不得超過支援的上限

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|原則|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的文字。

## <a name="issue"></a>問題

*伺服器設定了太多邏輯處理器。*

## <a name="impact"></a>影響

*Microsoft 不支援在這部電腦上執行 Hyper-v。*

## <a name="resolution"></a>解決方案

*請從此電腦移除一些處理器，或使用 msconfig 來限制可用的處理器數目。*

請參閱下列指示以使用 MSConfig。 如需移除處理器的詳細資訊，請參閱電腦隨附的指示，或洽詢硬體製造商。 如需 Hyper-v 支援的最大設定的詳細資訊，請參閱 [規劃 Windows Server 2016 中的 hyper-v 擴充性](../plan/plan-hyper-v-scalability-in-windows-server.md)。

### <a name="to-limit-the-number-of-available-processors"></a>限制可用處理器的數目

1.  開啟 System Configuration app ( # A0) 。 若要這樣做，請按一下 [ **開始**]，輸入 **msconfig**，以滑鼠右鍵按一下 **系統** 設定桌面應用程式，然後按一下 [以系統 **管理員身分執行**]。

2.  在 [ **開機** ] 索引標籤中，按一下 [ **Advanced options**]。

3.  選取 [ **處理器數目** ]，然後在清單中選取一個數位。 按一下 [確定]。

4.  重新開機電腦，以使用新的處理器數目來執行它。