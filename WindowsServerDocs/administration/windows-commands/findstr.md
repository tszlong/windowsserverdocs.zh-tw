---
title: findstr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c2d803fb-4cd2-46a1-a1b7-6f5e0249c418
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82ba51cdb49501492c1fa38c6c93933f4aee90d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890459"
---
# <a name="findstr"></a>findstr



搜尋模式的檔案中的文字。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
findstr [/b] [/e] [/l | /r] [/s] [/i] [/x] [/v] [/n] [/m] [/o] [/p] [/f:<File>] [/c:<String>] [/g:<File>] [/d:<DirList>] [/a:<ColorAttribute>] [/off[line]] <Strings> [<Drive>:][<Path>]<FileName>[ ...]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/b|如果在一行的開頭，比對的文字模式。|
|/e|在行尾，比對的文字模式。|
|/l|處理序會解譯為常值搜尋字串。|
|/r|處理程序做為規則運算式搜尋字串。 此為預設設定。|
|/s|會搜尋目前的目錄和所有子目錄。|
|/i|搜尋字串時，會忽略的字元大小寫。|
|/x|列印完全相符的行。|
|/v|列印不包含相符的行。|
|/n|列印 比對每一行的行號。|
|/m|如果檔案包含相符項目，會列印只有檔案名稱。|
|/o|列印每個相符的行前面的字元位移。|
|/p|會略過非列印字元的檔案。|
|/off[line]|不會略過具有離線的屬性集的檔案。|
|/f:\<檔案 >|取得檔案清單，從指定的檔案。|
|/c:\<字串 >|使用指定的文字當做常值搜尋字串。|
|/g:\<檔案 >|取得搜尋從指定的檔案的字串。|
|/d:\<DirList>|搜尋指定的目錄清單。 每個目錄必須以分號 （;），例如`dir1;dir2;dir3`。|
|/a:\<ColorAttribute >|指定使用兩個十六進位數字的色彩屬性。 型別`color /?`如需詳細資訊。|
|\<Strings>|指定要在 搜尋文字*FileName*。 必要。|
|[\<Drive>:][<Path>]<FileName>[ ...]|指定的位置和檔案或要搜尋的檔案。 需要至少一個檔案名稱。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   所有**findstr**前面必須加上命令列選項*字串*並*FileName*命令字串中。
-   規則運算式使用常值字元以及中繼字元來尋找模式的文字，而不是確切的字元的字串。 常值字元是規則運算式語法中沒有特殊意義的字元，它會比對所出現的字元。 比方說，字母和數字是常值字元。 中繼字元在規則運算式語法中是具有特殊意義 （操作員或分隔符號） 的符號。

    下表列出的 metacharacter， **findstr**接受。  
    |中繼字元|值|
    |-------------|-----|
    |.|任何字元的萬用字元：|
    |*|重複： 零個或多個發生次數的前一個字元或類別|
    |^|行位置： 行首|
    |$|行位置： 行尾|
    |[class]|字元類別： 一組中的任何一個字元|
    |[^class]|反向的類別： 不在集合中任何一個字元|
    |[x-y]|範圍： 內的任何字元指定的範圍|
    |\x|逸出： 中繼字元 x 的常值的用法|
    |\\<string|文字的位置： 字首|
    |string\>|文字的位置： 字尾|

    一起使用時，規則運算式語法中的特殊字元會有最多電力。 例如，使用下列萬用字元的字元 （.） 的組合，並重複 （*） 字元來比對任何字元字串：  
    ```
    .*
    ```  
    使用下列運算式做為較大運算式的一部分，來比對任何以"b"開頭和結尾為"ing"的字串：  
    ```
    b.*ing
    ```

## <a name="BKMK_examples"></a>範例

使用空格來分隔多個搜尋字串，除非前面加上引數 **/c**。

若要搜尋的"hello"或"there"檔案 x.y 中輸入：
```
findstr "hello there" x.y 
```
若要搜尋 「 哈囉 」 檔案 x.y 中，輸入：
```
findstr /c:"hello there" x.y 
```
若要尋找所有出現的單字"Windows"（使用大寫字母 W） Proposal.txt 檔案中，輸入：
```
findstr Windows proposal.txt 
```
若要搜尋目前目錄和包含 word Windows，不管字母大小寫的所有子目錄中的每個檔案中，輸入：
```
findstr /s /i Windows *.* 
```
若要尋找所有出現的行開頭為"FOR"，並會加上零個或多個空格 （如所示的電腦程式迴圈），並顯示找到每個出現的行號，輸入：
```
findstr /b /n /r /c:"^ *FOR" *.bas 
```
若要搜尋的一組檔案中的多個字串，建立文字檔，其中包含每個搜尋準則，在不同行上。 您也可以列出您想要搜尋的文字檔案中的實際檔案。 比方說，若要使用的搜尋準則 Stringlist.txt 檔案中，搜尋 Filelist.txt 中, 列出的檔案，然後再將結果儲存在檔案 Results.out，型別︰
```
findstr /g:stringlist.txt /f:filelist.txt > results.out 
```
若要列出包含單字 「 電腦 」，在目前的目錄和所有子目錄，不區分大小寫，每個檔案中，輸入：
```
findstr /s /i /m "\<computer\>" *.*
```
若要列出包含 「 電腦 」 和任何其他文字開頭為 「 元件 」，（例如"讚美 」 和 「 佔用 」） 類型的每個檔案：
```
findstr /s /i /m "\<comp.*" *.*
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)