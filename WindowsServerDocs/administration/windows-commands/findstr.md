---
title: findstr
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97cc58d2b87190c43137e8b193f0217fb98c006c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725607"
---
# <a name="findstr"></a>findstr

搜尋檔案中的文字模式。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/b|符合文字模式（如果它位於行首）。|
|/e|符合文字模式（如果它位於行尾）。|
|/l|逐文書處理搜尋字串。|
|/r|將搜尋字串當做正則運算式處理。 這是預設值。|
|/s|搜尋目前目錄和所有子目錄。|
|/i|搜尋字串時，忽略字元的大小寫。|
|/x|列印完全相符的行。|
|/v|只列印不包含相符項的行。|
|/n|列印每一行符合的行號。|
|/m|只有在檔案包含相符的檔案時，才會列印檔案名。|
|/o|在每個相符的行之前列印字元位移。|
|/p|略過具有不可列印字元的檔案。|
|/off [行]|不會略過已設定離線屬性的檔案。|
|/f：\<File>|從指定的檔案取得檔案清單。|
|/c：\<字串>|使用指定的文字做為常值搜尋字串。|
|/g：\<File>|從指定的檔案取得搜尋字串。|
|/d：\<DirList>|搜尋指定的目錄清單。 每個目錄都必須以分號分隔（例如`dir1;dir2;dir3`，;)。|
|/a：\<ColorAttribute>|指定具有兩個十六進位數位的色彩屬性。 如`color /?`需其他資訊，請輸入。|
|\<字串>|指定要在*FileName*中搜尋的文字。 必要。|
|[\<磁片磁碟機>：][<Path>]<FileName>[ ...]|指定要搜尋的位置和檔案。 至少需要一個檔案名。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

- 所有的**findstr**命令列選項都必須在命令字串中的*字串*和*檔案名*之前。
- 正則運算式會使用常值字元和元字元來尋找文字的模式，而不是精確的字元字串。 常值字元是在正則運算式語法中沒有特殊意義的字元，它符合該字元的出現次數。 例如，字母和數位是常值字元。 「元字元」是一種符號，在正則運算式語法中具有特殊意義（運算子或分隔符號）。

  下表列出**findstr**接受的元字元。  

  |元字元|值|
  |-------------|-----|
  |.|萬用字元：任何字元|
  |*|重複：上一個或多個字元或類別出現零次以上|
  |^|行位置：行首|
  |$|行位置：行尾|
  |課堂|字元類別：集合中的任何一個字元|
  |[^ 類別]|反向類別：不在集合中的任何一個字元|
  |[x-y]|範圍：指定範圍內的任何字元|
  |\x|Escape：使用元字元 x 的常值|
  |\\<字串|文字位置：字組開頭|
  |string\>|文字位置：字尾|

  當您一起使用時，正則運算式語法中的特殊字元具有最大的功能。 例如，使用下列萬用字元組合（.）並重複（*）字元來比對任何字元字串：

  ```
  .*
  ``` 

  使用下列運算式做為較大運算式的一部分，以比對開頭為 b 並以 ing 結尾的任何字串： 

  ```
  b.*ing
  ```

## <a name="examples"></a>範例

除非引數前面加上 **/c**，否則請使用空格來分隔多個搜尋字串。

若要搜尋 hello 或 file x. y，請輸入：

```
findstr hello there x.y 
```

若要在 file x. y 中搜尋 hello，請輸入：

```
findstr /c:hello there x.y 
```

若要在檔案提案 .txt 中尋找所有出現的 word 視窗（包含初始大寫字母 W），請輸入：

```
findstr Windows proposal.txt 
```

若要搜尋目前目錄中的每個檔案以及包含 word Windows 的所有子目錄，不論字母大小寫為何，請輸入：

```
findstr /s /i Windows *.* 
```

若要尋找所有以為開頭，且前面加上零或多個空格（如同在電腦程式迴圈中）的所有專案，並顯示每次找到的行號，請輸入：

```
findstr /b /n /r /c:^ *FOR *.bas 
```

若要在一組檔案中搜尋多個字串，請建立一個文字檔，其中包含個別行上的每個搜尋條件。 您也可以列出想要在文字檔中搜尋的確切檔案。 例如，若要使用 Stringlist 檔案中的搜尋準則，請搜尋 Filelist 中列出的檔案，然後將結果儲存在檔案結果中，輸入：

```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```

若要列出在目前目錄和所有子目錄中包含 word computer 的每個檔案，請在不區分大小寫的情況下，輸入：

```
findstr /s /i /m \<computer\> *.*
```

若要列出包含「電腦」和開頭為「複合」之任何其他單字的每個檔案（例如「補充和競爭」），請輸入：

```
findstr /s /i /m \<comp.* *.*
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)