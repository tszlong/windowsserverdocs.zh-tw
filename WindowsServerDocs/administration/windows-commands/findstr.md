---
title: findstr
description: Findstr 命令的參考文章，可搜尋檔案中的文字模式。
ms.topic: reference
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 65223132d4577a5e90929073cb964851f1ab67ce
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637550"
---
# <a name="findstr"></a>findstr

搜尋檔案中的文字模式。

## <a name="syntax"></a>語法

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<file>] [/c:<string>] [/g:<file>] [/d:<dirlist>] [/a:<colorattribute>] [/off[line]] <strings> [<drive>:][<path>]<filename>[ ...]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /b | 如果文字模式位於行的開頭，則會比對。 |
| /e | 符合文字模式（如果它位於行尾）。 |
| /l | 以真正的程式處理搜尋字串。 |
| /r | 以正則運算式處理搜尋字串。 這是預設值。 |
| /s | 搜尋目前的目錄和所有子目錄。 |
| /i | 搜尋字串時，忽略字元的大小寫。 |
| /x | 列印完全相符的行。 |
| /v | 只列印不包含相符項的行。 |
| /n | 列印每一行相符的行號。 |
| /m | 如果檔案包含相符的檔案，則只列印檔案名。 |
| /o | 在每個相符行之前列印字元位移。 |
| /p | 略過具有不可列印字元的檔案。 |
| /off [行] | 不會略過已設定 offline 屬性的檔案。 |
| /f`<file>` | 從指定的檔案取得檔案清單。 |
| /c`<string>` | 使用指定的文字做為常值搜尋字串。 |
| /g`<file>` | 從指定的檔案取得搜尋字串。 |
| /d`<dirlist>` | 搜尋指定的目錄清單。 每個目錄都必須以分號 (; 例如 ) `dir1;dir2;dir3` 。 |
| /a`<colorattribute>` | 指定具有兩個十六進位數位的色彩屬性。 輸入 `color /?` 以取得詳細資訊。 |
| `<strings>` | 指定要在 *檔案名*中搜尋的文字。 必要。 |
| `[\<drive>:][<path>]<filename>[ ...]` | 指定要搜尋的位置和檔案。 至少需要一個檔案名。 |
| /? | 在命令提示字元顯示 [說明]。 |

#### <a name="remarks"></a>備註

- 所有 **findstr** 命令列選項都必須在命令字串中的 *字串* 和 *檔案名* 之前。

- 正則運算式會使用常值字元和中繼字元來尋找文字的模式，而不是確切的字元字串。

  - 「常值字元」（literal character）是在正則運算式語法中沒有特殊意義的字元;相反地，它會符合該字元的出現次數。 例如，字母和數位都是常值字元。

  - 中繼字元是具有特殊意義的符號， (在正則運算式語法中) 運算子或分隔符號。

    接受的元字元是：

    | 元字元 | 值 |
    | -------------- | ----- |
    | `.` | **萬用字元** -任何字元 |
    | `*` | **重複** 零次或多次出現之前的字元或類別。 |
    | `^` | **開始行位置** -行首。 |
    | `$` | **結束行位置** -行尾。 |
    | `[class]` | **字元類別** -集合中的任何一個字元。 |
    | `[^class]` | **反向類別** -任何一個不在集合中的字元。 |
    | `[x-y]` | **範圍** -指定範圍內的任何字元。 |
    | `\x` | 中繼字元的**Escape**常值使用。 |
    | `<string` | **開始文字位置** -單字開頭。 |
    | `string>` | **結束文字位置** 結尾。 |

    當您一起使用時，正則運算式語法中的特殊字元具有最強大的功能。 例如，使用萬用字元 (的組合 `.`) 並重複 (`*`) 字元，以符合任何字元字串： `.*`

    使用下列運算式做為較大運算式的一部分，以比對開頭為 *b* 的任何字串，並以 *ing*結尾： `b.*ing`

- 若要在一組檔案中搜尋多個字串，您必須建立一個文字檔，將每個搜尋準則包含在個別的行上。

- 除非引數前面加上 **/c**，否則請使用空格來分隔多個搜尋字串。

### <a name="examples"></a>範例

若要搜尋 *hello* 或 *file* *x. y*，請輸入：

```
findstr hello there x.y
```

若要搜尋檔案*x. y*中的*hello* ，請輸入：

```
findstr /c:hello there x.y
```

若要使用檔案*proposal.txt*中的初始大寫字母（) W）來尋找所有出現的 word *Windows* (，請輸入：

```
findstr Windows proposal.txt
```

若要搜尋目前目錄中的每個檔案，以及所有包含 word *視窗*的子目錄（不論字母大小寫為何），請輸入：

```
findstr /s /i Windows *.*
```

若要找出所有開頭 *為的* 行，並在前面加上零或多個空格 (如同電腦程式迴圈中的) ，以及顯示找到每個出現的行號，請輸入：

```
findstr /b /n /r /c:^ *FOR *.bas
```

若要列出您想要在文字檔中搜尋的確切檔案，請使用 [檔案 *stringlist.txt*] 中的搜尋條件，以搜尋 *filelist.txt*中所列的檔案，然後將結果儲存在檔案 *結果中。輸出*之後，請輸入：

```
findstr /g:stringlist.txt /f:filelist.txt > results.out
```

若要列出目前目錄和所有子目錄中包含文字 *電腦* 的每個檔案（不論大小寫為何），請輸入：

```
findstr /s /i /m <computer> *.*
```

若要列出每個包含 word 電腦的檔案，以及任何其他以 comp 開頭的單字， (例如補充和競爭) ，請輸入：

```
findstr /s /i /m <comp.* *.*
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)