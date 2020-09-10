---
title: ftype
description: Ftype 命令的參考文章，此命令會顯示或修改副檔名關聯中使用的檔案類型。
ms.topic: reference
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: db5781eccb4fc54fea42586b5e7aab779509bffd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636654"
---
# <a name="ftype"></a>ftype

顯示或修改檔案名副檔名關聯中使用的檔案類型。 如果在沒有指派運算子的情況下使用 (=) ，此命令會顯示指定之檔案類型的目前開啟命令字串。 如果使用時不含參數，此命令會顯示已定義開啟命令字串的檔案類型。

> [!NOTE]
> 只有 cmd.exe 中才支援此命令，而且無法從 PowerShell 使用。
> 不過，您可以使用 `cmd /c ftype` 做為因應措施。

## <a name="syntax"></a>語法

```
ftype [<filetype>[=[<opencommandstring>]]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<filetype>` | 指定要顯示或變更的檔案類型。 |
| `<opencommandstring>` | 指定開啟指定檔案類型的檔案時，所要使用的開啟命令字串。|
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

下表說明 **ftype** 如何在開啟的命令字串內取代變數：

| 變數 | 取代值 |
| -------- | ----------------- |
| `%0` 或 `%1` | 取代為透過關聯啟動的檔案名。 |
| `%*` | 取得所有參數。 |
| `%2`, `%3`, ... | 取得第一個參數 (`%2`) ，第二個參數 (`%3`) 等等。 |
| `%~<n>` | 從第 *n*個參數開始取得所有剩餘的參數，其中 *n* 可以是介於2到9之間的任何數位。 |

### <a name="examples"></a>範例

若要顯示目前已定義開啟命令字串的檔案類型，請輸入：

```
ftype
```

若要顯示 *txtfile* 檔案類型目前開啟的命令字串，請輸入：

```
ftype txtfile
```

這個命令產生的輸出類似下面所述：

`txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1`

若要刪除稱為 *範例*之檔案類型的開啟命令字串，請輸入：

```
ftype example=
```

若要建立 pl 副檔名與 PerlScript 檔案類型的關聯性，並啟用 PerlScript 檔案類型以執行 PERL.EXE，請輸入下列命令：

```
assoc .pl=PerlScript
ftype PerlScript=perl.exe %1 %*
```

若要避免在叫用 Perl 腳本時輸入 pl 副檔名的需求，請輸入：

```
set PATHEXT=.pl;%PATHEXT%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
