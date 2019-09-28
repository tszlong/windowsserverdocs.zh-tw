---
title: title
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42094e0f1231fee5ac9ef0ec9184ba685c8846b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385789"
---
# <a name="title"></a>title



建立 [命令提示字元] 視窗的標題。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
title [<String>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<String >|指定 [命令提示字元] 視窗的標題。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要建立 batch 程式的視窗標題，請在 batch 程式開頭包含**標題**命令。
-   設定視窗標題之後，您只能使用 [**標題**] 命令來重設它。

## <a name="BKMK_examples"></a>典型

在下列範例腳本中，當批次檔執行**複製**命令時，[命令提示字元] 視窗的標題會變更為 [正在更新檔案]。 執行命令之後，會顯示文字 `Files Updated`，而且 [命令提示字元] 視窗的標題會變更回 [命令提示字元]。
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)