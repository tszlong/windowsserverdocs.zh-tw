---
title: 使用中的邏輯處理器數目不得超過支援的最大值
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 4a78f81fa90bc25d9ca1888d2c74d90a417f1071
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993431"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>使用中的邏輯處理器數目不得超過支援的最大值

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

## <a name="resolution"></a>解決方法

*請從這部電腦移除一些處理器，或使用 msconfig 來限制可用的處理器數目。*

請參閱下列指示以使用 Msconfig。 如需移除處理器的詳細資訊，請參閱電腦隨附的指示，或洽詢硬體製造商。 如需 Hyper-v 支援的最大設定的詳細資訊，請參閱[在 Windows Server 2016 中規劃 hyper-v 擴充性](../plan/plan-hyper-v-scalability-in-windows-server.md)。

### <a name="to-limit-the-number-of-available-processors"></a>限制可用的處理器數目

1.  開啟系統組態應用程式 ( # A0) 。 若要這麼做，請按一下 [**開始**]，輸入**msconfig**，以滑鼠右鍵按一下**系統**設定桌面應用程式，然後按一下 [**以系統管理員身分執行**]。

2.  在 [**開機**] 索引標籤上，按一下 [ **Advanced options**]。

3.  選取 [**處理器數目**]，然後在清單中選取一個數位。 按一下 [確定]  。

4.  重新開機電腦，以使用新的處理器數目來執行。