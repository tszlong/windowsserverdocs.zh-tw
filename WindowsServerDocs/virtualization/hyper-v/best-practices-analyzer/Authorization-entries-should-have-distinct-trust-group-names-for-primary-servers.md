---
title: 對於具有不屬於相同信任群組之虛擬機器的主伺服器，授權專案應具有不同的信任組名
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53cb81e0071dee09feffb6e37f2ab6dbece27d28
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970005"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>對於具有不屬於相同信任群組之虛擬機器的主伺服器，授權專案應具有不同的信任組名

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*伺服器將接受複本虛擬機器的複寫要求，從與虛擬機器相同複寫標記相關聯的授權清單中的任何伺服器。*

## <a name="impact"></a>**影響**
*當虛擬機器接受來自屬於不同授權專案之主伺服器的複寫時，可能會有隱私權和安全性考慮。這會影響下列授權專案：\<list of authorization entries>*

## <a name="resolution"></a>**解決方案**
*在主伺服器的授權專案中使用不同的標記，而這些虛擬機器不屬於相同的安全性群組。修改 Hyper-v 設定以設定複寫標記。*



