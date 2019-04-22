---
title: assoc
description: 適用於 Windows 命令主題**assoc** -顯示或修改檔案名稱副檔名關聯。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d893167081b66c81366b59613c52182a4ddba370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816639"
---
# <a name="assoc"></a>assoc



顯示或修改檔案名稱副檔名關聯。 如果未指定參數，使用**assoc**會顯示一份所有目前的檔案名稱副檔名關聯。

> [!NOTE]
> 此命令僅適用於在 cmd。EXE，而且無法從 PowerShell 使用。
>

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|<.ext>|指定的檔案名稱副檔名。|
|\<FileType>|指定要與指定的副檔名產生關聯的檔案類型。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要移除副檔名的檔案類型關聯，新增以泛空白字元等號之後按空格鍵。
-   若要檢視目前已開啟的命令字串所定義的檔案類型，請使用**ftype**命令。
-   輸出重新導向**assoc**到文字檔中，請使用**>** 重新導向運算子。

## <a name="BKMK_examples"></a>範例

若要檢視目前的檔案類型關聯的檔案名稱副檔名為.txt，請輸入：
```
assoc .txt
```
若要移除檔案名稱副檔名為.bak 的檔案類型關聯，請輸入：
```
assoc .bak= 
```

> [!NOTE]
> 請務必在等號後面加上空格。

若要檢視的輸出**assoc**一次輸入一個畫面：
```
assoc | more
```
若要傳送的輸出**assoc**檔案 assoc.txt 中，輸入：
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
