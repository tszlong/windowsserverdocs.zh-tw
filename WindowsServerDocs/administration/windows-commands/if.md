---
title: if
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9751dfe3e0cb0965cc2c5169ea19e0f08110b0ff
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438106"
---
# <a name="if"></a>if



在批次程式，會執行條件式處理。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

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

## <a name="parameters"></a>參數

|        參數        |                                                                                                                                                                                                                描述                                                                                                                                                                                                                 |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           否           |                                                                                                                                                                              指定的命令執行只有當條件為 false。                                                                                                                                                                              |
|  errorlevel\<數字 >   |                                                                                                                                                      指定的則為 true 的條件，才執行 Cmd.exe 的前一個程式傳回的結束代碼，等於或大於*數字*。                                                                                                                                                       |
|       \<命令 >        |                                                                                                                                                                            指定應該執行如果符合上述條件的命令。                                                                                                                                                                             |
|  \<String1>==<String2>  |                                                                                                             指定，則為 true 的條件才*String1*並*String2*都相同。 這些值可以是常值字串或批次的變數 (例如，%1)。 您不需要以引號括住的常值字串。                                                                                                              |
|    存在\<檔名 >    |                                                                                                                                                                                       如果指定的檔案名稱存在，請指定，則為 true 的條件。                                                                                                                                                                                        |
|      \<CompareOp>       |                                                                               指定三個字母的比較運算子。 下列清單顯示有效值*CompareOp*:</br>**EQU**等於</br>**NEQ**不等於</br>**LSS**小於</br>**LEQ**小於或等於</br>**GTR**大於</br>**GEQ**大於或等於                                                                                |
|           /i            |                                                            強制執行字串比較時忽略大小寫。  您可以使用 **/i**上<em>String1</em> **==** <em>String2</em>形式**如果**。 這些是一般的比較，在中，如果兩個*String1*並*String2*數字所組成，將字串轉換成數字，而且會執行數值的比較。                                                            |
| cmdextversion\<數字 > | 指定，則為 true 的條件才 Cmd.exe 的功能是相等的命令延伸模組相關聯或大於指定的數字的內部版本號碼。 第一個版本是 1。 顯著的增強功能新增至命令延伸模組時，它會以其中一個的遞增值增加。 **Cmdextversion**條件永遠不會為 true，則是當命令延伸模組已停用 （根據預設，命令會啟用擴充功能）。 |
|   定義\<變數 >   |                                                                                                                                                                                            如果指定條件為真*變數*定義。                                                                                                                                                                                            |
|      \<運算式 >      |                                                                                                                                                                   指定要傳遞至命令中的 命令列命令和任何參數**其他**子句。                                                                                                                                                                   |
|           /?            |                                                                                                                                                                                                    在命令提示字元顯示說明。                                                                                                                                                                                                    |

## <a name="remarks"></a>備註

-   如果在指定的條件**如果**子句為 true 時，執行命令所示的條件。如果條件為 false 中的命令**如果**子句會被忽略，命令會執行任何命令中指定**else**子句。
-   當程式停止時，它會傳回結束代碼。 若要當作條件使用結束代碼，請使用**errorlevel**。
-   如果您使用**定義**，下列三個變數新增到環境： **%errorlevel%** ， **cmdcmdline %** ，和 **%cmdextversion**.  
    -   **%errorlevel%** 展開成 ERRORLEVEL 環境變數的目前值的字串表示。 這是假設沒有現有的環境變數，其名稱 ERRORLEVEL，如果沒有，您會收到該 ERRORLEVEL 值。
    -   **%cmdcmdline%** 展開成原始的任何處理之前由 Cmd.exe 傳遞至 Cmd.exe 命令列。 這是假設沒有現有的環境變數，其名稱 CMDCMDLINE — 如果沒有，您會得到 CMDCMDLINE 的值。
    -   **%cmdextversion**展開目前的值的字串表示成**cmdextversion**。 這是假設沒有現有的環境變數，其名稱 CMDEXTVERSION — 如果沒有，您會得到 CMDEXTVERSION 的值。
-   您必須使用**else**命令之後的同一行上的子句**如果**。

## <a name="BKMK_examples"></a>範例

若要顯示訊息 「 找不到資料檔 」 檔案找不到 Product.dat，如果輸入：
```
if not exist product.dat echo Cannot find data file 
```
若要格式化磁碟機 A 中的磁碟，並顯示錯誤訊息，在格式化程序期間發生錯誤時，輸入下列命令列批次檔中：
```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```
若要從目前的目錄中刪除檔案 Product.dat 或顯示一則訊息，如果找不到 Product.dat，輸入下列命令列批次檔中：
```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> 這幾行可以結合成單一行，如下所示：
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```
> 若要執行批次檔之後，回應將 ERRORLEVEL 環境變數的值，輸入下列命令列批次檔中：
> ```
> goto answer%errorlevel%
> :answer1
> echo Program had return code 1
> :answer0
> echo Program had return code 0
> goto end
> :end
> echo Done! 
> ```
> 若要將 ERRORLEVEL 環境變數的值是否小於或等於 1，型別，請前往 「 好 」 的標籤：
> ```
> if %errorlevel% LEQ 1 goto okay
> ```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[If](if.md)

[移至](goto.md)