---
title: xcopy
description: Xcopy 的參考主題，其會複製檔案和目錄，包括子目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: eba7092a9a26b25b1fe77b39b8098d117b38981a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820998"
---
# <a name="xcopy"></a>xcopy

複製檔案和目錄，包括子目錄。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|\<來源>|必要。 指定您想要複製之檔案的位置和名稱。 這個參數必須包含磁片磁碟機或路徑。|
|[ \< 目的地>]|指定您想要複製之檔案的目的地。 這個參數可以包含磁碟機號和冒號、目錄名稱、檔案名或這些的組合。|
|/W|會顯示下列訊息，並在開始複製檔案之前等候您的回應：</br>**按任意鍵以開始複製檔案**|
|/p|提示您確認是否要建立每個目的地檔案。|
|/C|忽略錯誤。|
|/v|會在將每個檔案寫入目的地檔案時進行驗證，以確保目的地檔案與來源檔案相同。|
|/q|隱藏**xcopy**訊息的顯示。|
|/f|複製時顯示來源和目的地檔案名。|
|/l|顯示要複製的檔案清單。|
|/g|當目的地不支援加密時，建立解密的*目的地*檔案。|
|/d [： MM-DD-YYYY]|複製在指定的日期之前或之後變更的來源檔案。 如果您未包含*MM DD-YYYY*值， **xcopy**會複製比現有*目的地*檔案更新的所有*原始*程式檔。 此命令列選項可讓您更新已變更的檔案。|
|/U|只從存在於*目的地*的*來源*複製檔案。|
|/i|如果*來源*是目錄，或包含萬用字元且*目的地*不存在， **xcopy**會假設*目的地*指定目錄名稱，並建立新的目錄。 然後， **xcopy**會將所有指定的檔案複製到新的目錄。 根據預設， **xcopy**會提示您指定*目的地*是檔案或目錄。|
|/s|複製目錄和子目錄，除非它們是空的。 如果您省略 **/s**， **xcopy**會在單一目錄中運作。|
|/e|複製所有子目錄，即使它們是空的。 使用 **/e**搭配 **/s**和 **/t**命令列選項。|
|/t|只複製子目錄結構（也就是樹狀目錄），而不是檔案。 若要複製空的目錄，您必須包含 **/e**命令列選項。|
|/k|複製檔案，並在*目的地*檔案上保留唯讀屬性（如果存在於*來源*檔案）。 根據預設， **xcopy**會移除唯讀屬性。|
|/r|複製唯讀檔案。|
|/h|複製具有隱藏和系統檔案屬性的檔案。 根據預設， **xcopy**不會複製隱藏或系統檔案|
|/a|只複製已設定封存檔案屬性的*原始*程式檔。 **/a**不會修改來源檔案的封存檔案屬性。 如需如何使用**attrib**來設定封存檔案屬性的詳細資訊，請參閱[其他參考](#additional-references)資料。|
|/m|複製已設定封存檔案屬性的*原始*程式檔。 不同于 **/a**， **/m**會關閉來源所指定檔案中的封存檔案屬性。 如需如何使用**attrib**來設定封存檔案屬性的詳細資訊，請參閱[其他參考](#additional-references)資料。|
|/n|使用 NTFS 短檔案或目錄名稱來建立複本。 當您從 NTFS 磁片區將檔案或目錄複寫到 FAT 磁片區，或*目的地*檔案系統上需要 FAT 檔案系統命名慣例（也就是8.3 字元）時，就需要 **/n** 。 *目的地*檔案系統可以是 FAT 或 NTFS。|
|/o|複製檔案擁有權和任意存取控制清單（DACL）資訊。|
|/x|複製檔案 audit 設定和系統存取控制清單（SACL）資訊（意指 **/o**）。|
|/exclude： FileName1 [+ [FileName2] [+ [FileName3] （ \) ]|指定檔案的清單。 至少必須指定一個檔案。 每個檔案都會包含搜尋字串，其中每個字串都位於檔案中的個別行上。</br>當任何字串符合要複製之檔案絕對路徑的任何部分時，該檔案將會被排除而無法複製。 例如，指定字串**obj**將會排除目錄**obj**底下的所有檔案，或包含 **.obj**副檔名的所有檔案。|
|/y|隱藏提示確認您想要覆寫現有的目的檔案。|
|/-y|提示確認您想要覆寫現有的目的檔案。|
|/z|在可重新開機的模式下透過網路複製。|
|/b|複製符號連結，而不是檔案。 此參數是在 Windows Vista®中引進。|
|/j|複製沒有緩衝處理的檔案。 建議用於非常大型的檔案。 這個參數已在 Windows Server 2008 R2 中新增。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 使用 **/z**

  如果您在複製階段中斷連接（例如，如果伺服器離線連接），則在您重新建立連線之後，它會繼續。 **/z**也會顯示每個檔案完成複製作業的百分比。

- 在 COPYCMD 環境變數中使用 **/y** 。

  您可以在 COPYCMD 環境變數中使用 **/y** 。 您可以使用命令列上的 **/-y**來覆寫此命令。 根據預設，系統會提示您覆寫。

- 複製加密的檔案

  將加密的檔案複製到不支援 EFS 的磁片區時，會產生錯誤。 先將檔案解密，或將檔案複製到支援 EFS 的磁片區。

- 附加檔案

  若要附加檔案，請針對 [目的地] 指定單一檔案，但為 [來源] 指定多個檔案（也就是使用萬用字元或 file1 + file2 + file3 格式）。

- *目的地*的預設值

  如果您省略*目的地*， **xcopy**命令會將檔案複製到目前的目錄。

- 指定*目的地*是否為檔案或目錄

  如果*目的地*不包含現有的目錄，而且結尾不是反斜線（ \) ，則會出現下列訊息：

  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```

如果您想要將檔案複製到檔案，請按 F。 如果您想要將檔案複製到目錄，請按 D。

  您可以使用 **/i**命令列選項來隱藏此訊息，這會導致**xcopy**假設目的地是一個目錄（如果來源是一個以上的檔案或目錄）。
- 使用**xcopy**命令來設定*目的地*檔案的封存屬性

  **Xcopy**命令會建立具有 archive 屬性集的檔案，不論此屬性是否設定于原始程式檔中。 如需有關檔案屬性和**attrib**的詳細資訊，請參閱[其他參考](#additional-references)資料。

- 比較**xcopy**和**diskcopy**

  如果您的磁片包含子目錄中的檔案，而且您想要將它複製到具有不同格式的磁片，請使用**xcopy**命令，而不是**diskcopy**。 由於**diskcopy**命令會依曲目複製磁片追蹤，因此您的來源和目的地磁片必須具有相同的格式。 **Xcopy**命令沒有這項需求。 除非您需要完整的磁片映射複本，否則請使用**xcopy** 。

- **Xcopy**的結束代碼

  若要處理**xcopy**傳回的結束代碼，請在 batch 程式中的**if**命令列上使用**ErrorLevel**參數。 如需使用**if**處理結束代碼的 batch 程式範例，請參閱[其他參考](#additional-references)。 下表列出每個結束代碼和描述。

  |結束代碼|說明|
  |---------|-----------|
  |0|檔案已複製而不發生錯誤。|
  |1|找不到要複製的檔案。|
  |2|使用者按下 CTRL + C 以終止**xcopy**。|
  |4|發生初始化錯誤。 記憶體或磁碟空間不足，或您在命令列上輸入了不正確磁片磁碟機名稱或不正確語法。|
  |5|發生磁片寫入錯誤。|

## <a name="examples"></a>範例

**1.** 若要將磁片磁碟機 A 中的所有檔案和子目錄（包括任何空白子目錄）複製到磁片磁碟機 B，請輸入：

```
xcopy a: b: /s /e
```

**2.** 若要在上述範例中包含任何系統或隱藏的檔案，請新增<strong>/h</strong>命令列選項，如下所示：

```
xcopy a: b: /s /e /h
```

**3.** 若要更新 \Reports 目錄中的檔案，以及 \Rawdata 目錄中自1993年12月29日起變更的檔案，請輸入：

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** 若要更新上一個範例中所有存在於 \Reports 的檔案（不論日期為何），請輸入：

```
xcopy \rawdata \reports /u
```

**5.** 若要取得上一個命令所要複製的檔案清單（也就是不需要實際複製檔案），請輸入：

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

檔案 xcopy 會列出要複製的每個檔案。

**6.** 若要將 \Customer 目錄和所有子目錄複寫到 \\ \\ 網路磁碟機 h：上的目錄 Public\Address，請保留唯讀屬性，並在 H：上建立新檔案時提示您輸入：

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** 若要發出先前的命令，請確定**xcopy**會建立 \Address 目錄（如果不存在），並隱藏當您建立新目錄時所顯示的訊息，請新增 **/i**命令列選項，如下所示：

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** 您可以建立 batch 程式來執行**xcopy**作業，並在發生錯誤時使用批次**if**命令來處理結束代碼。 例如，下列 batch 程式會針對**xcopy**來源和目的地參數使用可取代的參數：

```
@echo off
rem COPYIT.BAT transfers all files in all subdirectories of
rem the source drive or directory (%1) to the destination
rem drive or directory (%2)
xcopy %1 %2 /s /e
if errorlevel 4 goto lowmemory
if errorlevel 2 goto abort
if errorlevel 0 goto exit
:lowmemory
echo Insufficient memory to copy files or
echo invalid drive or command-line syntax.
goto exit
:abort
echo You pressed CTRL+C to end the copy operation.
goto exit
:exit
```

若要使用上述的 batch 程式將 C:\Prgmcode 目錄及其子目錄中的所有檔案複製到磁片磁碟機 B，請輸入：

```
copyit c:\prgmcode b:
```

命令直譯器會替代 *%1*的**C:\Prgmcode**和**B：** *%2*，然後使用**xcopy**搭配 **/e**和 **/s**命令列選項。 如果**xcopy**遇到錯誤，則 batch 程式會讀取結束代碼，並移至適當的**IF ERRORLEVEL**語句中所指出的標籤，然後顯示適當的訊息並從 batch 程式結束。

**9.** 此範例所有非空白的目錄，加上其名稱符合以星號符號指定之模式的檔案。

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

在上述範例中，這個特定的來源參數值 **。 \\\*yml**會複製相同的3個檔案，即使已移除它的兩個路徑字元 **。 \\ ** 不過，如果已從來源參數中移除星號萬用字元，則不會複製任何檔案，而是只會將它設為 **。 \\yml**。

## <a name="additional-references"></a>其他參考

-   [複製](copy.md)
-   [移動](move.md)
-   [Dir](dir.md)
-   [Attrib](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [只有](if.md)
- [命令列語法關鍵](command-line-syntax-key.md)
