---
title: dir
description: Dir 命令的參考文章，此命令會顯示目錄的檔案和子目錄清單。
ms.topic: reference
ms.assetid: edcbf69b-eaa4-466e-b210-3dd8892f4d93
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c5edcbaac04d6f87721644fb4943e75456a21f66
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634989"
---
# <a name="dir"></a>dir

顯示目錄的檔案和子目錄清單。 如果使用時不含參數，此命令會顯示磁片的磁片區標籤和序號，後面接著磁片上的目錄和檔案清單 (包括其名稱，以及上次修改) 的日期和時間。 若為檔案，此命令會顯示名稱延伸和大小（以位元組為單位）。 此命令也會顯示所列出的檔案和目錄總數、其累計大小，以及磁片上剩餘的可用空間 (位元組數) 。

**Dir**命令也可以使用不同的參數，從 Windows 修復主控台執行。 如需詳細資訊，請參閱 [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

## <a name="syntax"></a>語法

```
dir [<drive>:][<path>][<filename>] [...] [/p] [/q] [/w] [/d] [/a[[:]<attributes>]][/o[[:]<sortorder>]] [/t[[:]<timefield>]] [/s] [/b] [/l] [/n] [/x] [/c] [/4] [/r]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[<drive>:][<path>]` | 指定您想要查看清單的磁片磁碟機和目錄。 |
| `[<filename>]` | 指定您想要查看清單的特定檔案或檔案群組。 |
| /p | 一次顯示清單的一個畫面。 若要查看下一個畫面，請按任意鍵。 |
| /q | 顯示檔案擁有權資訊。 |
| /W | 以寬格式顯示清單，每一行最多有五個檔案名或目錄名稱。 |
| /d | 以與 **/w**相同的格式顯示清單，但是檔案會依資料行排序。 |
| /a [[：] `<attributes>` ] | 只顯示具有指定屬性之目錄和檔案的名稱。 如果您未使用此參數，此命令會顯示隱藏和系統檔案以外的所有檔案的名稱。 如果您在未指定任何 *屬性*的情況下使用此參數，此命令會顯示所有檔案的名稱，包括隱藏的和系統檔案。 可能的 *屬性* 值清單如下：<ul><li>**d** -目錄</li><li>**h** -隱藏的檔案</li><li>**s** -系統檔案</li><li>**l** -重新剖析點</li><li>**r** -唯讀檔案</li><li>已準備好**封存的檔案**</li><li>**i** -不是內容索引檔案</li></ul>您可以使用這些值的任意組合，但不要使用空格來分隔值。 （選擇性）您可以使用冒號 (： ) 分隔符號，也可以使用連字號 ( ) 以表示「不」的前置詞。 例如，使用 **-s** 屬性不會顯示系統檔案。 |
| /o [[：] `<sortorder>` ] | 根據 *sortorder*（可以是下列值的任意組合）來排序輸出：<ul><li>**n** -依名稱字母順序</li><li>**e** -依擴充的字母順序</li><li>**g** -先群組目錄</li><li>**s** -依大小，最小的第一個</li><li>**d** -依日期/時間，最舊的第一個</li><li>使用 **-** 前置詞反轉排序次序</li></ul>系統會依照列出的順序來處理多個值。 請勿以空格分隔多個值，但您可以選擇性地使用冒號 (： ) 。<p>如果未指定 *sortorder* ， **dir/o** 會依字母順序列出目錄，後面接著檔案，這些檔案也會依字母順序排序。 |
| /t [[：] `<timefield>` ] | 指定要顯示或用於排序的時間欄位。 可用的 *timefield* 值為：<ul><li>**c** -建立</li><li>**a** -上次存取</li><li>**w** -上次寫入</li></ul> |
| /s | 列出指定目錄和所有子目錄中指定之檔案名的每一次。 |
| /b | 顯示目錄和檔案的簡略清單，不含其他資訊。 **/B**參數會覆寫 **/w**。 |
| /l | 使用小寫顯示未排序的目錄名稱和檔案名。 |
| /n | 顯示較長的清單格式，其中包含在螢幕最右邊的檔案名。 |
| /x | 顯示針對非8.3 檔案名產生的簡短名稱。 顯示與 **/n**的顯示相同，但簡短名稱是在長名稱之前插入。 |
| /C | 顯示檔案大小的千位分隔符號。 此為預設行為。 使用 **/c** 隱藏分隔符號。 |
| /4 | 以四位數格式顯示年份。 |
| /r | 顯示檔案的替代資料流程。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 若要使用多個 *檔案名* 參數，請使用空格、逗號或分號來分隔每個檔案名。

- 您可以使用萬用字元 (**&#42;** 或 **？**) ，以代表檔案名的一或多個字元，並顯示檔案或子目錄的子集。

- 您可以使用萬用字元（ **&#42;**）來取代任何字元字串，例如：

  - `dir *.txt` 列出目前目錄中副檔名開頭為 .txt 的所有檔案，例如 .txt、. txt1、txt_old。

  - `dir read *.txt` 列出目前目錄中以 read 開頭的所有檔案，以及副檔名以 .txt 開頭的檔案，例如 .txt、. txt1 或. txt_old。

  - `dir read *.*` 列出目前目錄中以任何副檔名開頭為 read 的所有檔案。

  星號萬用字元一律會使用簡短的檔案名對應，因此您可能會得到非預期的結果。 例如，下列目錄包含兩個檔案 ( # B0 2 和 t97.txt) ：

  ```
  C:\test>dir /x
  Volume in drive C has no label.
  Volume Serial Number is B86A-EF32

  Directory of C:\test

  11/30/2004  01:40 PM <DIR>  .
  11/30/2004  01:40 PM <DIR> ..
  11/30/2004  11:05 AM 0 T97B4~1.TXT t.txt2
  11/30/2004  01:16 PM 0 t97.txt
  ```

  您可能會預期輸入 `dir t97\*` 會傳回 t97.txt 的檔案。 但是，輸入會傳回 `dir t97\*` 這兩個檔案，因為星號萬用字元會使用其簡短名稱對應 *T97B4 ~1.TXT*，來比對檔案 t.txt2 到 t97.txt。 同樣地，鍵入 `del t97\*` 會刪除這兩個檔案。

- 您可以使用問號 (？ ) 替代名稱中的單一字元。 例如，輸入 `dir read???.txt` 會列出目前目錄中副檔名開頭為 read 的任何檔案，後面最多三個字元。 這包括 Read.txt、Read1.txt、Read12.txt、Read123.txt 和 Readme1.txt，但不是 Readme12.txt。

- 如果您在*屬性*中使用具有多個值的 **/a** ，此命令只會顯示具有所有指定屬性之檔案的名稱。 例如，如果您使用 **/a** 搭配 **r** 和 **-h** 作為屬性 (使用 `/a:r-h` 或 `/ar-h`) ，此命令只會顯示未隱藏之唯讀檔案的名稱。

- 如果您指定一個以上的 *sortorder* 值，此命令會依第一個準則和第二個準則來排序檔案名，依此類推。 例如，如果您使用 **/o** 搭配 **e** 和 **-s** 參數的 *sortorder* (藉由使用 `/o:e-s` 或 `/oe-s`) ，此命令會依副檔名排序目錄和檔案的名稱，其中最大的是第一個，然後顯示最終結果。 依副檔名排序的字母會導致檔案名，而不會先出現副檔名，然後再顯示目錄名稱和副檔名的檔案名。

- 如果您使用重新導向符號 (`>`) 將此命令的輸出傳送至檔案，或使用管道 (`|`) 將此命令的輸出傳送到另一個命令，則您必須使用 `/a:-d` 和 **/b** 來只列出檔案名。 您可以使用 *filename* 搭配 **/b** 和 **/s** 來指定此命令要搜尋目前目錄及其子目錄中所有符合 *檔案名*的檔案名。 此命令只會針對每個找到的檔案名，列出磁碟機號、目錄名稱、檔案名和副檔名 (每一行的一個路徑) ）。 使用管道將此命令的輸出傳送到另一個命令之前，您應該在 Autoexec nt 檔案中設定 *TEMP* 環境變數。

## <a name="examples"></a>範例

若要依字母順序顯示所有目錄，並以寬格式顯示，並在每個畫面之後暫停，請確定根目錄是目前的目錄，然後輸入：

```
dir /s/w/o/p
```

輸出會列出根目錄、子目錄和根目錄中的檔案，包括副檔名。 此命令也會列出樹狀目錄中每個子目錄的子目錄名稱和檔案名。

若要改變上述範例，讓 **dir** 顯示檔案名和副檔名，但省略目錄名稱，請輸入：

```
dir /s/w/o/p/a:-d
```

若要列印目錄清單，請輸入：

```
dir > prn
```

當您指定 **prn**時，會將目錄清單傳送到連接至 LPT1 埠的印表機。 如果您的印表機附加至不同的埠，您必須以正確埠的名稱取代 **prn** 。

您也可以藉由將**prn**取代為檔案名，將**dir**命令的輸出重新導向至檔案。 您也可以輸入路徑。 例如，若要將 **dir** 輸出導向 dir.doc 在 [記錄] 目錄中的檔案，請輸入：

```
dir > \records\dir.doc
```

如果 dir.doc 不存在， **dir** 會建立它，除非 **記錄** 目錄不存在。 在此情況下，會出現下列訊息：

```
File creation error
```

若要在磁片磁碟機 C 的所有目錄中顯示副檔名為 .txt 的所有檔案名清單，請輸入：

```
dir c:\*.txt /w/o/s/p
```

**Dir**命令會以寬格式顯示每個目錄中相符檔案名的字母順序清單，並且在每次畫面填滿時暫停，直到您按下任何鍵以繼續。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
