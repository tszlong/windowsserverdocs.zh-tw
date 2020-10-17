---
title: title
description: Title 命令的參考文章，此命令會建立命令提示字元視窗的標題。
ms.topic: reference
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f4aaaf198a898589699547c522a734e9e3e5aba2
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156676"
---
# <a name="title"></a>title

建立命令提示字元視窗的標題。

## <a name="syntax"></a>語法

```
title [<string>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<string>` | 指定要顯示為命令提示字元視窗標題的文字。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 若要建立 batch 程式的視窗標題，請在 batch 程式的開頭加入 **title** 命令。

- 設定視窗標題之後，您只能使用 **title** 命令將其重設。

## <a name="examples"></a>範例

若要在批次檔執行**copy**命令時將命令提示字元視窗標題變更為*更新*檔案，然後將標題傳回*命令提示*字元，請輸入下列腳本：

```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
