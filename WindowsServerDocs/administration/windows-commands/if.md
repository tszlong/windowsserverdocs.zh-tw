---
title: if
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e56624a5c82caefdf7cc51c7a84f67882756e274
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724864"
---
# <a name="if"></a>if



在 batch 程式中執行條件式處理。



## <a name="syntax"></a>語法

```
if [not] ERRORLEVEL <Number> <Command> [else <Expression>]
if [not] <String1>==<String2> <Command> [else <Expression>]
if [not] exist <FileName> <Command> [else <Expression>]
```
如果已啟用命令延伸模組，請使用下列語法：
```
if [/i] <String1> <CompareOp> <String2> <Command> [else <Expression>]
if cmdextversion <Number> <Command> [else <Expression>]
if defined <Variable> <Command> [else <Expression>]
```

### <a name="parameters"></a>參數

|        參數        |                                                                                                                                                                                                                描述                                                                                                                                                                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           否           |                                                                                                                                                                              指定只有在條件為 false 時，才執行命令。                                                                                                                                                                              |
|  errorlevel \<號碼>   |                                                                                                                                                      只有在 Cmd.exe 執行的先前程式傳回的結束代碼等於或大於*數位*時，才指定 true 條件。                                                                                                                                                       |
|       \<命令>        |                                                                                                                                                                            指定在符合前述條件時應執行的命令。                                                                                                                                                                             |
|  \<String1>= =<String2>  |                                                                                                             只有在*String1*和*String2*相同時，才指定 true 條件。 這些值可以是常值字串或批次變數（例如，%1）。 您不需要將常值字串括在引號中。                                                                                                              |
|    存在\<的檔案名>    |                                                                                                                                                                                       如果指定的檔案名存在，則指定 true 條件。                                                                                                                                                                                        |
|      \<CompareOp>       |                                                                               指定三個字母的比較運算子。 下列清單代表*CompareOp*的有效值：</br>**等於**等於</br>**NEQ**不等於</br>**LSS**小於</br>**LEQ**小於或等於</br>**GTR**大於</br>**GEQ**大於或等於                                                                                |
|           /i            |                                                            強制字串比較以忽略大小寫。  **如果**是，您可以使用<em>String1</em>**==**<em>string2</em>形式的 **/i** 。 這些比較是泛型的，在這種情況下，如果*String1*和*string2*都只由數位組成，則字串會轉換成數位，並執行數值比較。                                                            |
| cmdextversion \<號碼> | 只有當與 Cmd.exe 的命令延伸功能相關聯的內部版本號碼等於或大於指定的數位時，才指定 true 條件。 第一個版本是1。 當命令延伸模組中新增了顯著的增強功能時，它會增加一個增量。 當停用命令延伸模組時， **cmdextversion**條件一律為 true （預設會啟用命令延伸）。 |
|   定義\<的變數>   |                                                                                                                                                                                            如果已定義*變數*，則指定 true 條件。                                                                                                                                                                                            |
|      \<運算式>      |                                                                                                                                                                   指定命令列命令，以及要傳遞給**else**子句中命令的任何參數。                                                                                                                                                                   |
|           /?            |                                                                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                                                                    |

## <a name="remarks"></a>備註

-   如果在**if**子句中指定的條件為 true，則會執行在條件之後的命令。如果條件為 false，則會忽略**if**子句中的命令，而命令會執行**else**子句中指定的任何命令。
-   當程式停止時，它會傳回結束代碼。 若要使用結束代碼做為條件，請使用**errorlevel**。
-   如果您使用**已定義**的，則會將下列三個變數加入至環境： **% errorlevel%**、 **% cmdcmdline%** 和 **% cmdextversion%**。  
    -   **% errorlevel%** 會展開為 errorlevel 環境變數目前值的字串表示。 這會假設沒有名稱為 ERRORLEVEL 的現有環境變數（如果有的話），您會改為取得該 ERRORLEVEL 值。
    -   **% cmdcmdline%** 會擴展到在 cmd.exe 進行任何處理之前傳遞至 cmd.exe 的原始命令列。 這會假設沒有名為 CMDCMDLINE 的現有環境變數（如果有的話），您會改為取得 CMDCMDLINE 值。
    -   **% cmdextversion%** 會展開為目前**cmdextversion**值的字串表示。 這會假設沒有名為 CMDEXTVERSION 的現有環境變數（如果有的話），您會改為取得 CMDEXTVERSION 值。
-   在**if**之後，您必須在命令的同一行上使用**else**子句。

## <a name="examples"></a>範例

若要顯示 [找不到檔案時找不到資料檔案] 訊息，請輸入：
```
if not exist product.dat echo Cannot find data file 
```
若要格式化磁片磁碟機 A 中的磁片，並在格式化過程中發生錯誤時顯示錯誤訊息，請在批次檔中輸入下列幾行：
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
若要從目前目錄中刪除檔案產品，或在找不到產品 .dat 時顯示訊息，請在批次檔中輸入下列幾行：
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> 這些行可以合併成一行，如下所示：
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```
> 若要在執行批次檔之後回應 ERRORLEVEL 環境變數的值，請在批次檔中輸入下列幾行：
> ```
> goto answer%errorlevel%
> :answer1
> echo The program returned error level 1
> goto end
> :answer0
> echo The program returned error level 0
> goto end
> :end
> echo Done! 
> ```
> 若要移至 [ok] 標籤，如果 ERRORLEVEL 環境變數的值小於或等於1，請輸入：
> ```
> if %errorlevel% LEQ 1 goto okay
> ```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

[只有](if.md)

[Goto](goto.md)
