---
title: assoc
description: '[Assoc] 命令的參考主題，它會顯示或修改副檔名關聯。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f637e1f744ec412899320cfbb368633b222da8d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718956"
---
# <a name="assoc"></a>assoc

顯示或修改副檔名關聯。 如果使用時不含參數， **assoc**會顯示所有目前副檔名關聯的清單。

> [!NOTE]
> 只有在 CMD 中才支援此命令。EXE 和無法從 PowerShell 使用。

## <a name="syntax"></a>語法

```
assoc [<.ext>[=[<filetype>]]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<.ext>` | 指定副檔名。 |
| `<filetype>` | 指定要與指定的副檔名相關聯的檔案類型。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="remarks"></a>備註

- 若要移除副檔名的檔案類型關聯，請按空格鍵，在等號後面加上空白字元。

- 若要查看已定義 open 命令字串的目前檔案類型，請使用**ftype**命令。

- 若要將**assoc**的輸出重新導向至文字檔，請使用`>`重新導向運算子。

## <a name="examples"></a>範例

若要查看檔案名副檔名 .txt 的目前檔案類型關聯，請輸入：

```
assoc .txt
```

若要移除副檔名 .bak 的檔案類型關聯，請輸入：

```
assoc .bak= 
```

> [!NOTE]
> 請確定您在等號後面加上空格。

若要一次查看**assoc**一個畫面的輸出，請輸入：

```
assoc | more
```

若要將**assoc**的輸出傳送至檔案 assoc，請輸入：

```
assoc>assoc.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftype 命令](ftype.md)
