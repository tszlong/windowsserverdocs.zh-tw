---
title: 使用提供錯誤更正的 RAM
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 67eb6cef-b045-4748-90e1-406af5345d6a
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1f89ceb20cbd58260f103b244be80c9d8c292d93
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960331"
---
# <a name="use-ram-that-provides-error-correction"></a>使用提供錯誤更正的 RAM

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*在此電腦上使用的 RAM 不會錯誤修正 (ECC) RAM。*

## <a name="impact"></a>影響

*Microsoft 不支援電腦上的 Windows Server 2016，而不會錯誤修正 RAM。*

## <a name="resolution"></a>解決方法

*確認伺服器列在 Windows Server catalog 中，並且適用于 Hyper-v。*

若要檢查是否列出伺服器，請參閱[Windows server catalog](https://www.windowsservercatalog.com/)。



