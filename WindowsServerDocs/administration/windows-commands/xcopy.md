---
title: xcopy
description: Xcopy 的參考檔，它會複製檔案和目錄，包括子目錄。
ms.topic: reference
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 01/05/2019
ms.openlocfilehash: a3925843b80c60c689050c9a28236903e1912b3a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640671"
---
# <a name="xcopy"></a>xcopy

複製檔案和目錄（包括子目錄）。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Source>|必要。 指定您想要複製之檔案的位置和名稱。 此參數必須包含磁片磁碟機或路徑。|
|[\<Destination>]|指定您想要複製之檔案的目的地。 此參數可包含磁碟機號和冒號、目錄名稱、檔案名或這些的組合。|
|/W|會顯示下列訊息，並在開始複製檔案之前等候您的回應：</br>**按任意鍵以開始將檔案複製 (s) **|
|/p|提示您確認是否要建立每個目的地檔案。|
|/C|忽略錯誤。|
|/v|會在每個檔案寫入目的地檔案時進行驗證，以確定目的地檔案與原始程式檔相同。|
|/q|抑制 **xcopy** 訊息的顯示。|
|/f|在複製時顯示來源和目的地檔案名。|
|/l|顯示要複製的檔案清單。|
|/g|當目的地不支援加密時，會建立解密的 *目的地* 檔案。|
|/d [： MM-DD-YYYY]|複製來源檔案時，或只在指定的日期之後變更。 如果您未包含*MM DD-YYYY*值， **xcopy**會複製比現有的*目的地*檔案還要新的所有*來源*檔案。 此命令列選項可讓您更新已變更的檔案。|
|/U|從存在於*目的地*的*來源*複製檔案。|
|/i|如果 *來源* 是目錄或包含萬用字元，但 *目的地* 不存在，則 **xcopy** 會假設 *目的地* 指定目錄名稱，並建立新的目錄。 然後， **xcopy** 會將所有指定的檔案複製到新目錄中。 根據預設， **xcopy** 會提示您指定 *目的地* 為檔案或目錄。|
|/s|複製目錄和子目錄，除非它們是空的。 如果您省略 **/s**，則 **xcopy** 會在單一目錄中運作。|
|/e|複製所有子目錄（即使它們是空的）。 使用 **/e** 搭配 **/s** 和 **/t** 命令列選項。|
|/t|複製子目錄結構 (也就是僅) 的樹狀結構，而不是檔案。 若要複製空的目錄，您必須包含 **/e** 命令列選項。|
|/k|複製檔案，並保留 *目的地* 檔案的唯讀屬性（如果存在於 *來源* 檔案中）。 根據預設， **xcopy** 會移除唯讀屬性。|
|/r|複製唯讀檔案。|
|/h|複製具有隱藏和系統檔案屬性的檔案。 根據預設， **xcopy** 不會複製隱藏的檔案或系統檔案|
|/a|只複製已設定其封存檔案屬性的 *來源* 檔案。 **/a** 不會修改來源檔案的封存檔案屬性。 如需如何使用 **attrib**設定封存檔案屬性的詳細資訊，請參閱 [其他參考](#additional-references)。|
|/m|複製已設定其保存檔案屬性的 *原始* 程式檔。 與 **/a**不同的是， **/m** 會關閉來源中指定之檔案中的封存檔案屬性。 如需如何使用 **attrib**設定封存檔案屬性的詳細資訊，請參閱 [其他參考](#additional-references)。|
|/n|使用 NTFS 簡短檔案或目錄名稱來建立複本。 當您將檔案或目錄從 NTFS 磁片區複製到 FAT 磁片區，或當 FAT 檔案系統命名慣例 (也就是在*目的地*檔案系統上需要8.3 個) 字元時，就需要使用 **/n** 。 *目的地*檔案系統可以是 FAT 或 NTFS。|
|/o| (DACL) 資訊，複製檔案擁有權及任意存取控制清單。|
|/x|複製檔案審核設定和系統存取控制清單 (SACL) 資訊 (暗示 **/o**) 。|
|/exclude： FileName1 [+ [FileName2] [+ [FileName3] ( \) ]|指定檔案的清單。 至少必須指定一個檔案。 每個檔案都會包含搜尋字串，其中每個字串都在檔案的個別行上。</br>當任何字串符合要複製之檔案絕對路徑的任何部分時，就會將該檔案排除在複製之外。 例如，指定字串 **obj** 將會排除目錄 **obj** 下的所有檔案，或包含 **.obj** 副檔名的所有檔案。|
|/y|隱藏提示以確認您想要覆寫現有的目的地檔案。|
|/-y|提示確認您要覆寫現有的目的地檔案。|
|/z|在可重新開機模式的網路上複製。|
|/b|複製符號連結，而不是檔案。 此參數是在 Windows Vista®引進。|
|/j|複製檔案而不緩衝。 建議用於非常大型的檔案。 此參數是在 Windows Server 2008 R2 中新增的。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 使用 **/z**

  如果您在複製階段遺失連線 (例如，如果伺服器離線，則會在重新建立) 連線之後繼續進行。 **/z** 也會顯示每個檔案的複製作業已完成的百分比。

- 在 COPYCMD 環境變數中使用 **/y** 。

  您可以在 COPYCMD 環境變數中使用 **/y** 。 您可以使用命令列上的 **/-y** 覆寫此命令。 根據預設，系統會提示您進行覆寫。

- 複製加密的檔案

  將加密的檔案複製到不支援 EFS 的磁片區時，會產生錯誤。 先解密檔案，或將檔案複製到支援 EFS 的磁片區。

- 附加檔案

  若要附加檔案，請為目的地指定單一檔案，但為來源 (指定多個檔案，也就是使用萬用字元或 file1 + file2 + file3 格式) 。

- *目的地*的預設值

  如果您省略 *Destination*， **xcopy** 命令會將檔案複製到目前的目錄。

- 指定 *目的地* 是否為檔案或目錄

  如果 *目的地* 不包含現有的目錄，而且不是以反斜線 (結尾 \) ，則會出現下列訊息：

  ```
  Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
  ```

如果您想要將檔案複製到檔案中，請按 F。 如果您想要將檔案複製到目錄，請按 D。

  您可以使用 **/i** 命令列選項來隱藏此訊息，這會導致 **xcopy** 假設如果來源是一個以上的檔案或目錄，目的地是目錄。
- 使用 **xcopy** 命令設定 *目的地* 檔案的封存屬性

  無論此屬性是否已在原始程式檔中設定， **xcopy** 命令都會建立已設定 archive 屬性的檔案。 如需有關檔案屬性和 **attrib**的詳細資訊，請參閱 [其他參考](#additional-references)。

- 比較 **xcopy** 和 **diskcopy**

  如果您的磁片包含子目錄中的檔案，而且您想要將它複製到具有不同格式的磁片，請使用 **xcopy** 命令，而不是 **diskcopy**。 因為 **diskcopy** 命令會依曲目複製磁片追蹤，所以您的來源和目的地磁片必須具有相同的格式。 **Xcopy**命令沒有這項需求。 除非您需要完整的磁片映射複製，否則請使用 **xcopy** 。

- **Xcopy**的結束代碼

  若要處理**xcopy**傳回的結束代碼，請在 batch 程式的**if**命令列上使用**ErrorLevel**參數。 如需使用 **if**來處理結束代碼的 batch 程式範例，請參閱 [其他參考](#additional-references)。 下表列出每個結束代碼和描述。

  |結束碼|描述|
  |---------|-----------|
  |0|檔案已複製而沒有錯誤。|
  |1|找不到要複製的檔案。|
  |2|使用者按下 CTRL + C 以終止 **xcopy**。|
  |4|發生初始化錯誤。 沒有足夠的記憶體或磁碟空間，或您在命令列上輸入了不正確磁片磁碟機名稱或不正確語法。|
  |5|發生磁片寫入錯誤。|

## <a name="examples"></a>範例

**1.** 若要複製所有檔案和子目錄 (包括從磁片磁碟機 a 到磁片磁碟機 B) 的任何空白子目錄，請輸入：

```
xcopy a: b: /s /e
```

**2.** 若要在先前的範例中包含任何系統或隱藏的檔案，請新增<strong>/h</strong> 命令列選項，如下所示：

```
xcopy a: b: /s /e /h
```

**3.** 若要更新 \Reports 目錄中的檔案，並將 \Rawdata 目錄中的檔案自1993年12月29日起變更，請輸入：

```
xcopy \rawdata \reports /d:12-29-1993
```

**4.** 若要更新上一個範例中存在於 \Reports 中的所有檔案（不論日期為何），請輸入：

```
xcopy \rawdata \reports /u
```

**5.** 若要取得前一個命令所要複製的檔案清單 (也就是不需要實際複製檔案) ，請輸入：

```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```

File xcopy 會列出要複製的每個檔案。

**6.** 若要將 \Customer 目錄和所有子目錄複寫到 \\ \\ 網路磁碟機 H：上的目錄 Public\Address，請保留唯讀屬性，並在 H：上建立新檔案時提示您，輸入：

```
xcopy \customer h:\public\address /s /e /k /p
```

**7.** 若要發出先前的命令，請確定 **xcopy** 建立了 \Address 目錄（如果不存在），並隱藏在您建立新目錄時所出現的訊息，請新增 **/i** 命令列選項，如下所示：

```
xcopy \customer h:\public\address /s /e /k /p /i
```

**8.** 您可以建立 batch 程式來執行 **xcopy** 作業，並在發生錯誤時，使用 batch **if** 命令來處理結束代碼。 例如，下列 batch 程式會使用 **xcopy** 來源和目的地參數的可替換參數：

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

命令直譯器會將 *%1*和**B**的**C:\Prgmcode**取代為% *2*，然後將**xcopy**與 **/e**和 **/s**命令列選項搭配使用。 如果 **xcopy** 發生錯誤，批次程式就會讀取結束代碼，並移至適當的 **IF ERRORLEVEL** 語句中指出的標籤，然後顯示適當的訊息，並結束批次程式。

**9.** 這個範例會複製所有非空白的目錄，加上名稱符合以星號符號指定之模式的檔案。

```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```

在上述範例中，這個特定的來源參數值 **。 \\\*yml**會複製相同的3個檔案，即使它的兩個**路徑 \\ 字元都已移除**。 但是，如果已從來源參數移除星號萬用字元，就不會複製任何檔案，因此只會進行複製 **。 \\yml**。

## <a name="additional-references"></a>其他參考資料

- [複製](copy.md)
- [移動](move.md)
- [迪爾](dir.md)
- [Attrib](attrib.md)
- [Diskcopy](diskcopy.md)
- [If](if.md)
- [命令列語法關鍵](command-line-syntax-key.md)
