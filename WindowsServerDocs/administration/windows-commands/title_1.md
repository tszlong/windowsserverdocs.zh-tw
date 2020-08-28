---
title: title
description: 標題的參考文章，會建立 [命令提示字元] 視窗的標題。
ms.topic: reference
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20d0baf3c006fafd3ef6fb45a8cb69724c19929a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036066"
---
# <a name="title"></a>title

建立命令提示字元視窗的標題。



## <a name="syntax"></a>語法

```
title [<String>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<String>|指定命令提示字元視窗的標題。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要建立 batch 程式的視窗標題，請在 batch 程式的開頭加入 **title** 命令。
-   設定視窗標題之後，您只能使用 **title** 命令將其重設。

## <a name="examples"></a>範例

在下列範例腳本中，命令提示字元視窗的標題會在批次檔執行 **複製** 命令時變更為更新檔案。 執行命令之後， `Files Updated` 會顯示文字，並將命令提示字元視窗的標題變更回命令提示字元。
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)