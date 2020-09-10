---
title: fc
description: Fc 命令的參考文章，它會比較兩個檔案或一組檔案，並顯示兩者之間的差異。
ms.topic: reference
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 54a148ae7e722d891c3d8912c50c904839ddbf67
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634938"
---
# <a name="fc"></a>fc

比較兩個檔案或一組檔案，並顯示兩者之間的差異。

## <a name="syntax"></a>語法

```
fc /a [/c] [/l] [/lb<n>] [/n] [/off[line]] [/t] [/u] [/w] [/<nnnn>] [<drive1>:][<path1>]<filename1> [<drive2>:][<path2>]<filename2>
fc /b [<drive1:>][<path1>]<filename1> [<drive2:>][<path2>]<filename2>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /a | 縮寫 ASCII 比較的輸出。 **Fc**不會顯示所有不同的行，而是只會顯示每組差異的第一行和最後一行。 |
| /b | 比較二進位模式中的兩個檔案（位元組），而且不會在尋找不相符的情況時嘗試重新同步處理檔案。 這是比較具有下列副檔名之檔案的預設模式： .exe、.com、sys、.obj、.lib 或 bin。 |
| /C | 忽略字母大小寫。 |
| /l | 比較 ASCII 模式中的檔案、逐行，以及嘗試在找不相符的情況下重新同步處理檔案。 這是用來比較檔案的預設模式，但副檔名如下： .exe、.com、sys、.obj、.lib 或 bin。 |
| /lb`<n>` | 將內部行緩衝區的行數設定為 *N*。行緩衝區的預設長度是100行。 如果您要比較的檔案有超過100的連續不同行， **fc** 就會取消比較。 |
| /n | 顯示 ASCII 比較期間的行號。 |
| /off [行] | 不會略過已設定 offline 屬性的檔案。 |
| /t | 防止 **fc** 將定位字元轉換成空格。 預設行為是將定位字元視為空格，並在每八個字元位置停止。 |
| /U | 將檔案與 Unicode 文字檔做比較。 |
| /W | 壓縮空白字元 (也就是在比較期間) 的定位字元和空格。 如果行包含許多連續的空格或定位字元，則 **/w** 會將這些字元視為單一空格。 與 **/w**搭配使用時， **fc** 會忽略行開頭和結尾的空白字元。 |
| /`<nnnn>` | 指定在 **fc** 將檔案視為重新同步之前，必須符合下列各項的連續行數。 如果檔案中相符的行數小於 *nnnn*， **fc** 會將相符的行顯示為差異。 預設值為 2。 |
| `[<drive1>:][<path1>]<filename1>` | 指定要比較之第一個檔案或一組檔案的位置和名稱。 *filename1* 為必要項。 |
| `[<drive2>:][<path2>]<filename2>` | 指定要比較的第二個檔案或一組檔案的位置和名稱。 *filename2* 為必要項。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 此命令是 c:\WINDOWS\fc.exe implemeted。 您可以在 PowerShell 中使用此命令，但請務必將完整的可執行檔拼寫 ( # A0) ，因為 ' fc ' 也是格式自訂的別名。

- 當您使用 **fc** 進行 ASCII 比較時， **fc** 會依下列順序顯示兩個檔案之間的差異：

  - 第一個檔案的名稱

  - 不同檔案之間 *filename1* 的行數

  - 這兩個檔案中的第一行相符

  - 第二個檔案的名稱

  - 不同 *filename2* 的行

  - 要比對的第一行

- **/b** 以下列語法顯示二進位比較期間發現的不符：

    `\<XXXXXXXX: YY ZZ>`

    *Xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx*的值會指定位元組配對的相對十六進位位址，從檔案的開頭開始。 位址從00000000開始。 *YY*和*ZZ*的十六進位值分別代表*filename1*和*filename2*中不相符的位元組。

- 您可以在*filename1*和*filename2*中使用萬用字元 (**&#42;** 和 **？**) 。 如果您在 *filename1*中使用萬用字元， **fc** 會將所有指定的檔案與 *filename2*所指定的檔案或一組檔案進行比較。 如果您在 *filename2*中使用萬用字元， **fc** 會使用 *filename1*中的對應值。

- 比較 ASCII 檔案時， **fc** 會使用 (夠大的內部緩衝區來保存100行) 作為儲存體。 如果檔案大於緩衝區， **fc** 會比較它可以載入緩衝區的內容。 如果 **fc** 在檔案載入的部分中找不到相符的部分，則會停止並顯示下列訊息：

    `Resynch failed. Files are too different.`

    比較大於可用記憶體的二進位檔案時， **fc** 會完全比較兩個檔案，並將記憶體中的部分與磁片中的下一個部分覆迭。 輸出與完全符合記憶體的檔案相同。

### <a name="examples"></a>範例

若要對兩個文字檔進行 ASCII 比較（ *rpt* 和 *rpt*），並以縮寫格式顯示結果，請輸入：

```
fc /a monthly.rpt sales.rpt
```

若要對兩個批次檔進行二進位比較， *profits.bat* 和 *earnings.bat*，請輸入：

```
fc /b profits.bat earnings.bat
```

如下所示的結果：

```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
000005E8: 00 6E
FC: earnings.bat longer than profits.bat
```

如果 profits.bat 和 earnings.bat 檔案相同， **fc** 會顯示下列訊息：

```
Comparing files profits.bat and earnings.bat
FC: no differences encountered
```

若要將目前目錄中的每個 .bat 檔案與檔案 *new.bat*進行比較，請輸入：

```
fc *.bat new.bat
```

若要比較磁片磁碟機 C 上的檔案 *new.bat* 與磁片磁碟機 D 上的檔案 *new.bat* ，請輸入：

```
fc c:new.bat d:*.bat
```

若要將磁片磁碟機 C 上根目錄中的每個批次檔與磁片磁碟機 D 的根目錄中相同名稱的檔案進行比較，請輸入：

```
fc c:*.bat d:*.bat
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
