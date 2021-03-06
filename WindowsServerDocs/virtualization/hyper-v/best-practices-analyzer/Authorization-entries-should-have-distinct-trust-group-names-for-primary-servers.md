---
title: 對於具有不屬於相同信任群組之虛擬機器的主伺服器，授權專案應具有不同的信任組名
description: 瞭解當伺服器將在與虛擬機器相同的複寫標記關聯之授權清單中的任何伺服器接受複本虛擬機器時的複寫要求時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
ms.date: 8/16/2016
ms.openlocfilehash: 7d66212f0633608ca9d094316c18b5fdd311640e
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834733"
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

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*伺服器將會在與虛擬機器相同的複寫標籤相關聯之授權清單中的任何伺服器上，接受複本虛擬機器的複寫要求。*

## <a name="impact"></a>**影響**
*當虛擬機器接受來自屬於不同授權專案之主伺服器的複寫時，可能會有隱私權和安全性考慮。這會影響下列授權專案： \<list of authorization entries>*

## <a name="resolution"></a>**解決方法**
*使用不屬於相同安全性群組的虛擬機器，在主伺服器的授權專案中使用不同的標記。修改 Hyper-v 設定以設定複寫標記。*



