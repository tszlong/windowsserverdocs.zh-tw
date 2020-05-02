---
title: nslookup set class
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1ae3a5336815a5273aafa976b1dcad8b60fac9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723650"
---
# <a name="nslookup-set-class"></a>nslookup set class



變更查詢類別。 類別會指定資訊的通訊協定群組。

## <a name="syntax"></a>語法

```
set class=<Class>
```

### <a name="parameters"></a>參數

| 參數 |                                                                                                                                    描述                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<班級>  | 預設類別是在中。 以下列出此命令的有效值。</br>-IN：指定網際網路類別。</br>-混亂：指定混亂類別。</br>-HESIOD：指定 MIT Athena-io Hesiod 類別。</br>-ANY：指定先前列出的任何萬用字元。 |
|   {說明   |                                                                                                                                        ?}                                                                                                                                         |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)