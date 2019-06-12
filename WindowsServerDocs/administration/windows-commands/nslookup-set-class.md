---
title: nslookup set class
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7953f450c17afdee849515f8d8945631a30f4b98
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436847"
---
# <a name="nslookup-set-class"></a>nslookup set class



變更查詢類別。 此類別會指定通訊協定群組的資訊。

## <a name="syntax"></a>語法

```
set class=<Class>
```

## <a name="parameters"></a>參數

| 參數 |                                                                                                                                    描述                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<類別 >  | 預設類別是 in。 以下列出有效的值，這個命令。</br>在：指定網際網路類別。</br>-混亂：指定的 Chaos 類別。</br>-HESIOD:指定 MIT Athena Hesiod 類別。</br>-任何：指定任何先前所列的萬用字元。 |
|   {說明   |                                                                                                                                        ?}                                                                                                                                         |

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)