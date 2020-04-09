---
title: title
description: 標題的 Windows 命令主題，會建立 [命令提示字元] 視窗的標題。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0bbe8bd-201a-4b6c-b617-5d9809881dc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aae18e97daaef226443c5f18c6c401a4e2b82b59
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832801"
---
# <a name="title"></a>title

建立 [命令提示字元] 視窗的標題。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
title [<String>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<字串 >|指定 [命令提示字元] 視窗的標題。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要建立 batch 程式的視窗標題，請在 batch 程式開頭包含**標題**命令。
-   設定視窗標題之後，您只能使用 [**標題**] 命令來重設它。

## <a name="examples"></a><a name=BKMK_examples></a>典型

在下列範例腳本中，[命令提示字元] 視窗的標題會變更為 [更新檔案]，而批次檔會執行 [**複製**] 命令。 執行命令之後，會顯示 `Files Updated` 文字，而且 [命令提示字元] 視窗的標題會變更回 [命令提示字元]。
```
@echo off
title Updating Files
copy \\server\share\*.xls c:\users\common\*.xls
echo Files Updated.
title Command Prompt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)