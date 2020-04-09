---
title: fc
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b358b8c1bf44b5b7942cef05bd09fa8cac850a3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844741"
---
# <a name="fc"></a>fc



比較兩個檔案或一組檔案，並顯示兩者之間的差異。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

### <a name="parameters"></a>參數

|            參數             |                                                                                                                                     描述                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 縮寫 ASCII 比較的輸出。 **Fc**不會顯示不同的所有線條，而是只會針對每一組差異顯示第一個和最後一行。                                                  |
|                /b                |             比較兩個二進位模式的檔案（以位元組為單位），而且不會在發現不相符的情況下嘗試重新同步處理檔案。 這是比較具有下列副檔名之檔案的預設模式： .exe、.com、sys.databases、.obj、.lib 或 bin。              |
|                /c                |                                                                                                                               忽略字母大小寫。                                                                                                                               |
|                /l                |               比較 ASCII 模式中的檔案，逐行，並嘗試在發現不相符的情況下重新同步處理檔案。 這是比較檔案的預設模式，但副檔名為下列的檔案除外： .exe、.com、sys.databases、.obj、.lib 或 bin。                |
|             /lb\<N >              |                         將內部行緩衝區的行數設定為*N*。行緩衝區的預設長度為100行。 如果您要比較的檔案超過100個連續的不同行， **fc**會取消比較。                         |
|                /n                |                                                                                                                在 ASCII 比較期間顯示行號。                                                                                                                 |
|            /off [行]            |                                                                                                               不會略過已設定離線屬性的檔案。                                                                                                               |
|                /t                |                                                                    防止**fc**將索引標籤轉換為空格。 預設行為是將定位字元視為空格，並在每個第八個字元位置停止。                                                                    |
|                /u                |                                                                                                                        將檔案與 Unicode 文字檔做比較。                                                                                                                         |
|                /w                |         在比較期間壓縮空白字元（也就是定位字元和空格）。 如果一行包含多個連續的空格或索引標籤，則 **/w**會將這些字元視為單一空格。 與 **/w**搭配使用時， **fc**會忽略行開頭和結尾的空白字元。         |
|             /\<NNNN >             | 指定遵循不相符時必須符合的連續行數， **fc**會考慮重新同步處理檔案。 如果檔案中相符的行數小於*NNNN*， **fc**會顯示相符的行做為差異。 預設值為2。 |
| [\<Drive1 >：][<Path1>]<FileName1> |                                                                                        指定要比較之第一個檔案或一組檔案的位置和名稱。 *FileName1*是必要的。                                                                                        |
| [\<Drive2 >：][<Path2>]<FileName2> |                                                                                       指定要比較的第二個檔案或一組檔的位置和名稱。 *FileName2*是必要的。                                                                                        |
|                /?                |                                                                                                                         在命令提示字元顯示說明。                                                                                                                         |

## <a name="remarks"></a>備註

-   此命令是由 c:\WINDOWS\fc.exe. implemeted 您可以在 PowerShell 中使用此命令，但請務必將完整的可執行檔（eseutil.exe）拼出，因為 ' fc ' 是格式自訂的別名。

-   報告 ASCII 比較的檔案之間的差異

    當您使用**fc**進行 ASCII 比較時， **fc**會以下列順序顯示兩個檔案之間的差異：  
    -   第一個檔案的名稱
    -   *FileName1*中不同檔案的行
    -   要在兩個檔案中相符的第一行
    -   第二個檔案的名稱
    -   不同*FileName2*的線條
    -   要比對的第一行
-   使用 **/b**進行二進位比較

    **/b**會顯示在下列語法中進行二進位比較時所發現的不符：

    `\<XXXXXXXX: YY ZZ>`

    *Xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*的值會指定位元組配對的相對十六進位位址，從檔案的開頭開始測量。 位址開始于00000000。 *YY*和*ZZ*的十六進位值分別代表*FileName1*和*FileName2*中不相符的位元組。
-   使用萬用字元

    您可以在 **&#42;** *FileName1*和*FileName2*中使用萬用字元（和 **？** ）。 如果您在*FileName1*中使用萬用字元， **fc**會將所有指定的檔案與*FileName2*所指定的檔案或一組檔案進行比較。 如果您在*FileName2*中使用萬用字元， **Fc**會使用*FileName1*中的對應值。
-   使用記憶體

    比較 ASCII 檔案時， **fc**會使用內部緩衝區（夠大，足以容納100行）做為儲存空間。 如果檔案大於緩衝區， **fc**會比較它可以載入到緩衝區的內容。 如果**fc**在檔案載入的部分中找不到相符項，它會停止並顯示下列訊息：

    `Resynch failed. Files are too different.`

    比較大於可用記憶體的二進位檔案時， **fc**會完全比較這兩個檔案，並將記憶體中的部分與磁片中的下一個部分覆迭。 輸出與完全符合記憶體的檔案相同。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要進行兩個文字檔的 ASCII 比較（每月 rpt 和 rpt），並以縮寫的格式顯示結果，請輸入：
```
fc /a monthly.rpt sales.rpt 
```
若要進行兩個批次檔的二進位比較，請輸入：
```
fc /b profits.bat earnings.bat
```
結果如下所示：
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
如果獲利和收益 .bat 檔案相同， **fc**會顯示下列訊息：
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
若要比較目前目錄中的每個 .bat 檔案與檔案 .bat，請輸入：
```
fc *.bat new.bat
```
若要將 C 磁片磁碟機上的檔案 .bat 與磁片磁碟機 D 上的檔案 .bat 進行比較，請輸入：
```
fc c:new.bat d:*.bat
```
若要將磁片磁碟機 C 的根目錄中的每個批次檔與磁片磁碟機 D 的根目錄中相同名稱的檔案進行比較，請輸入：
```
fc c:*.bat d:*.bat
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
