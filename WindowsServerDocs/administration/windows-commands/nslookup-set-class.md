---
title: nslookup set class
description: Nslookup set class 命令的參考文章，此命令會變更查詢類別。
ms.topic: reference
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5f4eb309c3972230247bf231b0e731e146d8159a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634032"
---
# <a name="nslookup-set-class"></a>nslookup set class

變更查詢類別。 類別會指定資訊的通訊協定群組。

## <a name="syntax"></a>語法

```
set class=<class>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<class>` | 有效的值包括：<ul><li>**在中：** 指定網際網路類別。 這是預設值。</li><li>**混亂：** 指定混亂類別。</li><li>**HESIOD：** 指定 MIT Athena-io Hesiod 類別。</li><li>**任何：** 指定使用任何先前列出的值。</li></ul> |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
