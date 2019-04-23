---
title: title
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d1d1ea70849c3beb4503edfdaa5116384c14a2fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848499"
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
|\<String>|指定的命令提示字元 視窗的標題。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要建立批次程式的視窗標題，包括**title**命令批次程式的開頭。
-   設定視窗標題之後，您可以使用重設它僅**title**命令。

## <a name="BKMK_examples"></a>範例

在下列的範例指令碼，[命令提示字元] 視窗的標題變更為 「 正在更新檔案 」 的批次檔執行時**複製**命令。 執行命令之後，文字`Files Updated`隨即顯示，且 [命令提示字元] 視窗的標題會變更回 [命令提示字元]。
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)