---
title: 避免將一個儲存體路徑對應到多個資源集區
description: 瞭解當儲存體檔案路徑對應到多個資源集區時該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
ms.date: 8/16/2016
ms.openlocfilehash: f39ea4da5a7b966a29b0ad943d40b02556c9460d
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834633"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>避免將一個儲存體路徑對應到多個資源集區

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*儲存體檔案路徑會對應至多個資源集區。*

## <a name="impact"></a>**影響**
*針對指定的儲存集區類型，下列父系和子集區會共用相同的儲存體路徑：*

\<list of pools>

## <a name="resolution"></a>**解決方法**
*使用 Windows PowerShell 重新設定儲存體資源集區，讓多個集區不會使用相同的儲存體路徑。*



