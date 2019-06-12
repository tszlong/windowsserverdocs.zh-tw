---
title: forfiles
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: d5ac95b795d1c5a59f8917bf851ab08fb4d7c1e7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439179"
---
# <a name="forfiles"></a>forfiles



選取並執行命令，在檔案或一組檔案。 此命令可用於批次處理。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>參數

|                     參數                      |                                                                                                                                                                                                                                                                                                    描述                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<Path>                     |                                                                                                                                                                                                                                                 指定要從中開始搜尋的路徑。 根據預設，搜尋目前工作目錄中啟動。                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask>                  |                                                                                                                                                                                                                                                           根據指定的搜尋遮罩搜尋檔案。 預設搜尋遮罩 **\*。\\** \*.                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   會指示**forfiles**到子目錄以遞迴方式搜尋命令。                                                                                                                                                                                                                                                                    |
|                  /c "\<Command>"                   |                                                                                                                                                                                                                                  每個檔案上執行指定的命令。 命令字串應該以括住。 預設命令 **"cmd /c echo @file"** 。                                                                                                                                                                                                                                   |
| /d&nbsp;[{+\|-}]&#8288;[{\<Date>\|&#8288;\<Days>}] | 選取指定的時間範圍內的上次修改日期的檔案。</br>-選取檔案的上次修改日期晚於或等於 ( **+** ) 或早於或等於 ( **-** ) 指定的日期，其中*日期*是 MM/DD/YYYY 的格式。</br>-選取檔案的上次修改日期晚於或等於 ( **+** ) 的目前日期加上天數指定，或早於或等於 ( **-** ) 目前的日期減去天數指定。</br>-有效的值，如*天*包含 0 – 32，768 範圍中的任何數字。 如果指定沒有正負號，則 **+** 預設會使用。 |
|                         /?                         |                                                                                                                                                                                                                                                                                        在命令提示字元顯示說明。                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>備註

-   **Forfiles**最常用於批次檔。
-   **Forfiles /s**類似於**dir /s。**
-   您可以使用下列變數所指定的命令字串中 **/c**命令列選項。  

|變數|描述|
|--------|-----------|
|@FILE|檔案名稱。|
|@FNAME|不含副檔名的檔案名稱。|
|@EXT|檔案名稱的副檔名。|
|@PATH|檔案的完整路徑。|
|@RELPATH|檔案的相對路徑。|
|@ISDIR|如果檔案類型是目錄，將會評估為 TRUE。 否則，此變數會評估為 FALSE。|
|@FSIZE|檔案大小，以位元組為單位。|
|@FDATE|檔案的上次修改的日期戳記。|
|@FTIME|檔案的最後修改的時間戳記。|

-   具有**forfiles**，您可以在執行命令或引數傳遞至多個檔案。 例如，您可以執行**型別**命令樹狀目錄中副檔名為.txt 的檔案名稱中的所有檔案。 或您可以執行每個批次檔 (*.bat) 磁碟機 C 上的 檔案名稱"Myinput.txt 」 做為第一個引數。
-   具有**forfiles**，您可以執行下列任一項：  
    -   使用 選取檔案的絕對日期 或 相對日期 **/d**參數。
    -   使用變數，例如建置的檔案封存樹狀結構@FSIZE和@FDATE。
    -   區別目錄中的檔案，使用@ISDIR變數。
    -   在命令列中包含特殊字元，所使用的字元，在為 0x 的十六進位的程式碼*HH*格式 （例如，索引標籤的 0x09）。
-   **Forfiles**的運作方式是實作**recurse 子目錄**旗標設計來處理的單一檔案的工具。

## <a name="BKMK_examples"></a>範例

若要列出所有磁碟機 C 上的批次檔案，請輸入：
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
若要列出所有磁碟機 C 上的目錄，請輸入：
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
若要列出所有目前的目錄中至少一年的檔案，請輸入：
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
若要顯示的文字 」*檔案*已過期 」 早於 2007 年 1 月 1 日，檔案中目前的目錄中的每個輸入：
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
若要列出資料行格式，目前的目錄中的所有檔案的副檔名，並新增延伸模組之前的索引標籤，請輸入：
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
