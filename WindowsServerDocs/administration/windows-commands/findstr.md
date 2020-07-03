---
title: findstr
description: Findstr 命令的參考文章，它會搜尋檔案中的文字模式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0cf30f19ef23c1b3275b6b7632b03f0dd8e433a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931433"
---
# <a name="findstr"></a>findstr

搜尋檔案中的文字模式。

## <a name="syntax"></a>語法

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<file>] [/c:<string>] [/g:<file>] [/d:<dirlist>] [/a:<colorattribute>] [/off[line]] <strings> [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /b | 符合文字模式（如果它位於行首）。 |
| /e | 符合文字模式（如果它位於行尾）。 |
| /l | 逐文書處理搜尋字串。 |
| /r | 將搜尋字串當做正則運算式處理。 這是預設值。 |
| /s | 搜尋目前目錄和所有子目錄。 |
| /i | 搜尋字串時，忽略字元的大小寫。 |
| /x | 列印完全相符的行。 |
| /v | 只列印不包含相符項的行。 |
| /n | 列印每一行符合的行號。 |
| /m | 只有在檔案包含相符的檔案時，才會列印檔案名。 |
| /o | 在每個相符的行之前列印字元位移。 |
| /p | 略過具有不可列印字元的檔案。 |
| /off [行] | 不會略過已設定離線屬性的檔案。 |
| /f`<file>` | 從指定的檔案取得檔案清單。 |
| /c`<string>` | 使用指定的文字做為常值搜尋字串。 |
| /g`<file>` | 從指定的檔案取得搜尋字串。 |
| /d`<dirlist>` | 搜尋指定的目錄清單。 每個目錄都必須以分號分隔（例如，;) `dir1;dir2;dir3` 。 |
| /a`<colorattribute>` | 指定具有兩個十六進位數位的色彩屬性。 如需其他資訊，請輸入 `color /?` 。 |
| `<strings>` | 指定要在*filename*中搜尋的文字。 必要。 |
| `[\<drive>:][<path>]<filename>[ ...]` | 指定要搜尋的位置和檔案。 至少需要一個檔案名。 |
| /? | 在命令提示字元顯示 [說明]。 |

#### <a name="remarks"></a>備註

- 所有的**findstr**命令列選項都必須在命令字串中的*字串*和*檔案名*之前。

- 正則運算式會使用常值字元和元字元來尋找文字的模式，而不是精確的字元字串。

  - 常值字元是在正則運算式語法中沒有特殊意義的字元;相反地，它會符合該字元的出現次數。 例如，字母和數位是常值字元。

  - 中繼字元是一種符號，在正則運算式語法中具有特殊意義（運算子或分隔符號）。

    接受的中繼字元為：

    | 中繼字元 | 值 |
    | -------------- | ----- |
    | `.` | **萬用字元**-任何字元 |
    | `*` | **重複**-先前的一個或多個字元或類別出現零次或多次。 |
    | `^` | **開始行位置**-行首。 |
    | `$` | **結束行位置**-行尾。 |
    | `[class]` | **字元類別**-集合中的任何一個字元。 |
    | `[^class]` | **反向類別**-不在集合中的任何一個字元。 |
    | `[x-y]` | **範圍**-指定範圍內的任何字元。 |
    | `\x` | 使用中繼字元的**Escape**常值。 |
    | `<string` | **開始文字位置**-單字的開頭。 |
    | `string>` | **結束文字位置**-單字的結尾。 |

    當您一起使用時，正則運算式語法中的特殊字元具有最大的功能。 例如，使用萬用字元（ `.` ）和重複（）字元的組合， `*` 以符合任何字元字串：`.*`

    使用下列運算式做為較大運算式的一部分，以比對開頭為*b*並以*ing*結尾的任何字串：`b.*ing`

- 若要在一組檔案中搜尋多個字串，您必須建立一個文字檔，其中包含個別行上的每個搜尋條件。

- 除非引數前面加上 **/c**，否則請使用空格來分隔多個搜尋字串。

### <a name="examples"></a>範例

若要搜尋*hello*或*file* *x. y*，請輸入：

```
findstr hello there x.y
```

若要在 file *x. y*中搜尋*hello* ，請輸入：

```
findstr /c:hello there x.y
```

若要在檔案*proposal.txt*中尋找所有出現的 word*視窗*（包含初始大寫字母 W），請輸入：

```
findstr Windows proposal.txt
```

若要搜尋目前目錄中的每個檔案以及包含 word *Windows*的所有子目錄，不論字母大小寫為何，請輸入：

```
findstr /s /i Windows *.*
```

若要尋找所有以*為*開頭，且前面加上零或多個空格（如同在電腦程式迴圈中）的所有專案，並顯示每次找到的行號，請輸入：

```
findstr /b /n /r /c:^ *FOR *.bas
```

若要列出您想要在文字檔中搜尋的確切檔案，請使用檔案*stringlist.txt*中的搜尋條件，搜尋*filelist.txt*中列出的檔案，然後將結果儲存在檔案*結果中。輸出*，請輸入：

```
findstr /g:stringlist.txt /f:filelist.txt > results.out
```

若要列出在目前目錄和所有子目錄中包含 word *computer*的每個檔案，請在不區分大小寫的情況下，輸入：

```
findstr /s /i /m <computer> *.*
```

若要列出包含「電腦」和開頭為「複合」之任何其他單字的每個檔案（例如「補充和競爭」），請輸入：

```
findstr /s /i /m <comp.* *.*
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)