---
title: nslookup set class
description: Nslookup set class 命令的參考主題，它會變更查詢類別。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e939be13eedcab557dc6dcbe16f2e83f810c20d5
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721561"
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
| `<class>` | 有效值包括：<ul><li>**在中：** 指定網際網路類別。 這是預設值。</li><li>**混亂：** 指定混亂類別。</li><li>**HESIOD：** 指定 MIT Athena-io Hesiod 類別。</li><li>**任何：** 指定使用先前列出的任何值。</li></ul> |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
