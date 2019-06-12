---
title: fc
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7ccc3d268b58bfa5e848f2336f4315baae8b4e1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439317"
---
# <a name="fc"></a>fc



比較兩個檔案或檔案的設定，並顯示它們之間的差異。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>參數

|            參數             |                                                                                                                                     描述                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 縮寫的 ASCII 比較的輸出。 不會顯示所有的不同，行**fc**顯示只有第一個和最後一列每一組的差異。                                                  |
|                /b                |             比較兩個檔案，在二進位模式中，位元組，並不會嘗試重新同步處理之後發現不相符的檔案。 這是預設模式，來比較具有下列副檔名的檔案：.exe、.com、.sys、.obj、.lib 或.bin。              |
|                /c                |                                                                                                                               會忽略字母大小寫。                                                                                                                               |
|                /l                |               比較中的檔案 ASCII 模式、--行，以及嘗試重新同步處理之後發現不相符的檔案。 這是預設模式，來比較檔案，除了具有下列副檔名的檔案：.exe、.com、.sys、.obj、.lib 或.bin。                |
|             /lb\<N>              |                         設定的內部企業緩衝區的行數*N*。行緩衝區的預設長度是 100 行。 如果您要比較的檔案有 100 個以上連續的不同程式行**fc**取消比較。                         |
|                /n                |                                                                                                                ASCII 比較時，會顯示行號。                                                                                                                 |
|            /off[line]            |                                                                                                               不會略過具有離線的屬性集的檔案。                                                                                                               |
|                /t                |                                                                    防止**fc**將定位字元轉換為空格。 預設行為是將定位點視為空格，但每個第八個字元的位置停駐點。                                                                    |
|                /u                |                                                                                                                        比較檔案為 Unicode 文字檔案。                                                                                                                         |
|                /w                |         在比較時，會壓縮泛空白字元 （亦即，索引標籤和空格）。 如果一行包含許多的連續的空格或定位字元， **/w**這些字元視為單一空格。 當搭配 **/w**， **fc**會忽略泛空白字元開頭和結尾行。         |
|             /\<NNNN>             | 指定的之前必須符合下列不相符的連續行數**fc**會考慮要重新同步處理的檔案。 相符的行，在檔案中的數字是否小於*NNNN*， **fc**差異顯示相符的行。 預設值為 2。 |
| [\<Drive1>:][<Path1>]<FileName1> |                                                                                        指定的位置和名稱的第一個檔案或一組要比較的檔案。 *FileName1*需要。                                                                                        |
| [\<Drive2>:][<Path2>]<FileName2> |                                                                                       指定的位置和名稱的第二個檔案或一組要比較的檔案。 *FileName2*需要。                                                                                        |
|                /?                |                                                                                                                         在命令提示字元顯示說明。                                                                                                                         |

## <a name="remarks"></a>備註

-   此命令是由 c:\WINDOWS\fc.exe implemeted。 您可以使用此命令在 PowerShell 中，但務必拼出完整的可執行檔 (fc.exe)，因為 'fc' Format-custom 的別名。

-   報告以 ASCII 比較檔案之間的差異

    當您使用**fc**以 ASCII 的比較來說**fc**顯示依下列順序的兩個檔案之間的差異：  
    -   第一個檔案的名稱
    -   中的程式行會*FileName1* ，差異檔案
    -   這兩個檔案中要比對的第一行
    -   第二個檔案的名稱
    -   中的程式行會*FileName2*不同的
    -   若要比對的第一行
-   使用**b**二進位比較

    **b**顯示不相符二進位比較期間找到的語法如下：

    `\<XXXXXXXX: YY ZZ>`

    值*XXXXXXXX*指定的位元組，從檔案開頭算起的相對十六進位位址。 位址開始 00000000。 十六進位值*YY*並*ZZ*代表不相符的位元組，從*FileName1*並*FileName2*分別。
-   使用萬用字元

    您可以使用萬用字元 ( **&#42;** 並 **？** ) 中*FileName1*並*FileName2*。 如果您使用中的萬用字元*FileName1*， **fc**比較所有指定的檔案，檔案或一組所指定的檔案*FileName2*。 如果您使用中的萬用字元*FileName2*， **fc**會使用的對應值*FileName1*。
-   使用記憶體

    比較 ASCII 檔案時**fc**使用內部緩衝區 （足以容納 100 行） 做為儲存體。 如果檔案大於緩衝區**fc**比較它可以載入到緩衝區。 如果**fc**找不到相符項目中的檔案載入的部分，它會停止，並顯示下列訊息：

    `Resynch failed. Files are too different.`

    比較，大於可用的記憶體中，二進位檔案時**fc**比較這兩個檔案完全，重疊在記憶體中的部分，與從磁碟的下一個部分。 輸出會是相同的完全納入記憶體中的檔案。

## <a name="BKMK_examples"></a>範例

若要進行的兩個文字檔案，Monthly.rpt 和 Sales.rpt，ASCII 比較，並以縮寫格式顯示結果，請輸入：
```
fc /a monthly.rpt sales.rpt 
```
若要進行二進位比較的兩個批次檔，Profits.bat 和 Earnings.bat，輸入：
```
fc /b profits.bat earnings.bat
```
下列會顯示類似的結果：
```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
...
...
...
000005E8: 00 6E
FC: Earnings.bat longer than Profits.bat
```
如果將 Profits.bat 和 Earnings.bat 檔案相同， **fc**顯示下列訊息：
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
若要比較檔案 New.bat 的目前目錄中每個.bat 檔案，請輸入：
```
fc *.bat new.bat
```
若要比較的檔案 New.bat C 磁碟機上檔案 New.bat D 磁碟機上，輸入：
```
fc c:new.bat d:*.bat
```
若要比較每個批次檔的根目錄中檔案的 C 磁碟機上具有相同名稱的磁碟機 D 上的根目錄中，輸入：
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
