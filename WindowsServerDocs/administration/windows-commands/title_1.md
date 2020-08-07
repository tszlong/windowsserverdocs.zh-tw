---
title: title
description: 標題的參考文章，會建立 [命令提示字元] 視窗的標題。
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fede3a0f71da2913e798852817722eaea414770
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881394"
---
# <a name="title"></a>title

建立 [命令提示字元] 視窗的標題。



## <a name="syntax"></a>語法

```
title [<String>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<String>|指定 [命令提示字元] 視窗的標題。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要建立 batch 程式的視窗標題，請在 batch 程式開頭包含**標題**命令。
-   設定視窗標題之後，您只能使用 [**標題**] 命令來重設它。

## <a name="examples"></a>範例

在下列範例腳本中，[命令提示字元] 視窗的標題會變更為 [更新檔案]，而批次檔會執行 [**複製**] 命令。 執行命令之後， `Files Updated` 會顯示文字，而且 [命令提示字元] 視窗的標題會變更回 [命令提示字元]。
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)