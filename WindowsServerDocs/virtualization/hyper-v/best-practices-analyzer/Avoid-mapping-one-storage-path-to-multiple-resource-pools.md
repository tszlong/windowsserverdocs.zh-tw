---
title: 避免將一個儲存體路徑對應至多個資源集區
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 38e0fdb5956197984a78d195ea23a7e856634575
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942066"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>避免將一個儲存體路徑對應至多個資源集區

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*儲存體檔案路徑會對應至多個資源集區。*

## <a name="impact"></a>**影響**
*針對指定的存放集區類型，下列父系和子集區會共用相同的儲存體路徑：*

\<list of pools>

## <a name="resolution"></a>**解決方案**
*使用 Windows PowerShell 重新設定存放裝置資源集區，讓多個集區不會使用相同的儲存路徑。*



