---
title: ftype
description: Ftype 命令的參考文章，它會顯示或修改副檔名關聯中所使用的檔案類型。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab401c4dd4707cf05c69c1746368927c0bfaa83
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930461"
---
# <a name="ftype"></a>ftype

顯示或修改副檔名關聯中所使用的檔案類型。 如果在沒有指派運算子（=）的情況下使用，此命令會顯示指定檔案類型目前開啟的命令字串。 如果使用時不含參數，此命令會顯示已定義 open 命令字串的檔案類型。

> [!NOTE]
> 只有 cmd.exe 中才支援此命令，而且無法從 PowerShell 使用。
> 雖然您可以使用 `cmd /c ftype` 做為因應措施。

## <a name="syntax"></a>語法

```
ftype [<filetype>[=[<opencommandstring>]]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<filetype>` | 指定要顯示或變更的檔案類型。 |
| `<opencommandstring>` | 指定開啟指定檔案類型的檔案時，所要使用的 open 命令字串。|
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

下表描述**ftype**如何在開啟的命令字串中替代變數：

| 變數 | 取代值 |
| -------- | ----------------- |
| `%0` 或 `%1` | 會以透過關聯啟動的檔案名取代。 |
| `%*` | 取得所有參數。 |
| `%2`, `%3`, ... | 取得第一個參數（ `%2` ）、第二個參數（ `%3` ）等等。 |
| `%~<n>` | 取得所有開頭為第*n*個參數的剩餘參數，其中*n*可以是介於2到9之間的任何數位。 |

### <a name="examples"></a>範例

若要顯示目前已定義 open 命令字串的檔案類型，請輸入：

```
ftype
```

若要顯示*txtfile*檔案類型目前開啟的命令字串，請輸入：

```
ftype txtfile
```

這個命令產生的輸出類似下面所述：

`txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1`

若要刪除稱為*範例*之檔案類型的開啟命令字串，請輸入：

```
ftype example=
```

若要將 pl 副檔名與 PerlScript 檔案類型產生關聯，並讓 PerlScript 檔案類型 PERL.EXE 執行，請輸入下列命令：

```
assoc .pl=PerlScript
ftype PerlScript=perl.exe %1 %*
```

若要在叫用 Perl 腳本時不必輸入 pl 副檔名，請輸入：

```
set PATHEXT=.pl;%PATHEXT%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
