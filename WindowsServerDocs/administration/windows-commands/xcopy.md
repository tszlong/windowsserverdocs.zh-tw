---
title: xcopy
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76a310d7-9925-4571-a252-0e28960d5f89
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 01/05/2019
ms.openlocfilehash: 54697b1c967d3e21583977418383d5a372e6f5d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859399"
---
# <a name="xcopy"></a>xcopy



複製檔案和目錄，包括子目錄

如需如何使用此命令的範例，請參閱[範例](xcopy.md#BKMK_examples)

## <a name="syntax"></a>語法

```
Xcopy <Source> [<Destination>] [/w] [/p] [/c] [/v] [/q] [/f] [/l] [/g] [/d [:MM-DD-YYYY]] [/u] [/i] [/s [/e]] [/t] [/k] [/r] [/h] [{/a | /m}] [/n] [/o] [/x] [/exclude:FileName1[+[FileName2]][+[FileName3]] [{/y | /-y}] [/z] [/b] [/j]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<來源 >|必要。 指定您想要複製之檔案的名稱與位置。 這個參數必須包含的磁碟機或路徑。|
|[\<目的地 >]|指定您想要複製的檔案的目的地。 這個參數可以包含磁碟機代號與冒號、 目錄名稱、 檔案名稱或這些項目的組合。|
|/w|顯示下列訊息，並等候您的回應，之後才開始複製檔案：</br>**按任意鍵以開始複製檔案**|
|/p|會提示您確認您是否想要建立每個目的地檔案。|
|/c|忽略錯誤。|
|/v|在寫入至目的地檔案，請確定目的地檔案是與相同原始程式檔，請確認每個檔案。|
|/q|隱藏顯示**xcopy**訊息。|
|/f|在複製時，會顯示來源和目的地檔案名稱。|
|/l|顯示是要複製的檔案的清單。|
|/g|建立解密*目的地*檔案時的目的地不支援加密。|
|/d [:MM-DD-YYYY]|複製來源上，或只在指定日期之後變更的檔案。 如果您未包含*MM DD YYYY*的值， **xcopy**複製所有*來源*比現有檔案更新的檔案*目的地*檔案。 此命令列選項可讓您更新已變更的檔案。|
|/u|將從檔案複製*來源*存在於上*目的地*只。|
|/i|如果*來源*是目錄，或包含萬用字元並*目的地*不存在， **xcopy**假設*目的地*指定目錄名稱，並建立新的目錄。 然後， **xcopy**將所有指定的檔案複製到新的目錄。 根據預設， **xcopy**會提示您指定是否*目的地*是檔案或目錄。|
|/s|請將複製目錄和子目錄，除非其為空白。 如果您省略 **/s**， **xcopy**會在單一目錄中運作。|
|/e|即使是空白，請將複製所有的子目錄。 使用 **/e**具有 **/s**並 **/t**命令列選項。|
|/t|子目錄結構 （也就是樹狀目錄），而不複製檔案的複本。 若要複製空的目錄，您必須包含 **/e**命令列選項。|
|/k|複製檔案，並會保留上的唯讀屬性*目的地*檔案如果位於*來源*檔案。 根據預設， **xcopy**移除唯讀屬性。|
|/r|複製唯讀檔案。|
|/h|將檔案複製以隱藏和系統檔案屬性。 根據預設， **xcopy**複製隱藏或系統檔案|
|/a|只複製*來源*檔案具有其封存檔案的屬性集。 **/ a**不會修改原始程式檔的封存檔案屬性。 如需有關如何設定使用的封存檔案屬性資訊**attrib**，請參閱[其他參考](xcopy.md#BKMK_addref)。|
|/m|複本*來源*檔案具有其封存檔案的屬性集。 不同於 **/a**， **/m**關閉所指定來源中的檔案中的封存檔案屬性。 如需有關如何設定使用的封存檔案屬性資訊**attrib**，請參閱[其他參考](xcopy.md#BKMK_addref)。|
|/n|使用簡短的 NTFS 檔案或目錄名稱，以建立複本。 **/n**時將檔案複製，或在上必要的 FAT 磁碟區的 NTFS 磁碟區的目錄或 FAT 檔案系統的命名慣例 （也就是 8.3 個字元） 時，務必*目的地*檔案系統。 *目的地*可以 FAT 或 NTFS 檔案系統。|
|/o|複製檔案擁有權和判別存取控制清單 (DACL) 資訊。|
|/x|複製檔案稽核設定和系統存取控制清單 (SACL) 資訊 (意指 **/o**)。|
|/exclude:FileName1[+[FileName2][+[FileName3]( \)]|指定檔案的清單。 必須指定至少一個檔案。 每個檔案會包含搜尋字串，其中每個檔案中的個別行上的字串。</br>當其中任一字串比對要複製檔案的絕對任何的路徑部分時，該檔案會複製 excuded。 比方說，指定字串**obj**目錄下的所有檔案中都排除**obj**或使用的所有檔案 **.obj**延伸模組。|
|/y|不要提示您確認您想要覆寫現有目的地檔案。|
|/-y|若要確認您想要覆寫現有目的地檔案的提示。|
|/z|透過網路以可重新啟動模式的複本。|
|/b|複製符號連結，而不是檔案。 這個參數是在 Windows Vista® 引進。|
|/j|將檔案複製而不緩衝。 建議針對非常大型的檔案。 Windows Server 2008 R2 中，已新增這個參數。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用 **/z**

    如果您遺失您的連線 （例如，如果伺服器離線，就會切斷連線） 的複製期間，它將會繼續之後重新建立連線。 **/z**也會顯示完成每個檔案，複製作業的百分比。
-   使用 **/y** COPYCMD 環境變數中。

    您可以使用 **/y** COPYCMD 環境變數中。 您可以使用覆寫此命令 **/-y**命令列上。 根據預設，系統會提示您覆寫。
-   複製加密的檔案

    將加密的檔案複製到不支援在錯誤中的 EFS 結果的磁碟區。 第一次解密檔案或將檔案複製到支援 EFS 的磁碟區。
-   附加檔案

    若要附加的檔案，指定單一檔案的目的地，但多個檔案的來源 (亦即，使用萬用字元或 file1 + file2 + file3 格式)。
-   預設值*目的地*

    如果您省略*目的地*，則**xcopy**命令將檔案複製到目前的目錄。
-   指定是否*目的地*是檔案或目錄

    如果*目的地*不包含現有的目錄，且結尾不是反斜線 (\)，出現下列訊息：  
    ```
    Does <Destination> specify a file name or directory name on the target(F = file, D = directory)?
    ```  
    如果您想要複製到檔案，請按 F。 如果您想要複製到目錄的檔案，請按 D。

    您可以使用，以隱藏此訊息 **/i**命令列選項，這會導致**xcopy**假設目的地是一個目錄，如果來源是一個以上的檔案或目錄。
-   使用**xcopy**命令，以設定封存屬性*目的地*檔案

    **Xcopy**命令會建立檔案以封存屬性集，不論是否已將此屬性設定原始程式檔中。 如需有關檔案屬性及**attrib**，請參閱[其他參考](xcopy.md#BKMK_addref)。
-   比較**xcopy**和**diskcopy**

    如果您擁有包含檔案子目錄中的磁碟，而且您想要將它複製到磁碟，就會有不同的格式，請使用**xcopy**命令，而非**diskcopy**。 因為**diskcopy**命令會將複製磁碟追蹤所追蹤，您的來源和目的地磁碟必須有相同的格式。 **Xcopy**命令沒有這項需求。 使用**xcopy**除非您需要完整的磁碟映像複本。
-   結束代碼**xcopy**

    若要處理序所傳回的結束代碼**xcopy**，使用**ErrorLevel**參數**如果**批次程式中的命令列。 如需範例的批次程式處理序結束碼使用之**如果**，請參閱[其他參考](xcopy.md#BKMK_addref)。 下表列出每個結束代碼和描述。  
    |結束代碼|描述|
    |---------|-----------|
    |0|檔案已複製不會發生錯誤。|
    |1|找不到要複製的任何檔案。|
    |2|使用者按下 CTRL + C 來終止**xcopy**。|
    |4|發生初始化錯誤。 沒有足夠的記憶體或磁碟空間，或在命令列上輸入的是無效的磁碟機名稱或語法無效。|
    |5|磁碟寫入錯誤發生。|

## <a name="BKMK_examples"></a>範例

**1.** 若要將從磁碟機 A 的所有檔案和子目錄 （包含空的任何子目錄） 都複製到磁碟機 B，請輸入：
```
xcopy a: b: /s /e 
```
**2.** 若要在上述範例中包含的任何系統或隱藏的檔案，新增 **/h**命令列選項，如下所示：
```
xcopy a: b: /s /e /h
```
**3.** 若要更新 \Reports 目錄中的檔案與 \Rawdata 目錄中，自 1993 年 12 月 29，類型已變更的檔案：
```
xcopy \rawdata \reports /d:12-29-1993
```
**4.** 若要更新的所有檔案，在上述範例中，不論日期 \Reports 存在於輸入：
```
xcopy \rawdata \reports /u
```
**5.** 若要取得要由先前命令複製之檔案的清單 (也就是不實際將檔案複製)，型別：
```
xcopy \rawdata \reports /d:12-29-1993 /l > xcopy.out
```
檔案 xcopy.out 列出要複製的每個檔案。

**6.** 若要複製到目錄 \Customer 目錄和所有子目錄\\ \\Public\Address 網路磁碟機 h:、 保留的唯讀屬性，和 h，類型上建立新的檔案時，系統會提示：
```
xcopy \customer h:\public\address /s /e /k /p
```
**7.** 若要發行前一個命令，請確認**xcopy**建立 \Address 目錄，如果不存在，然後隱藏訊息出現時建立新的目錄中，新增 **/i**命令列選項，如下所示：
```
xcopy \customer h:\public\address /s /e /k /p /i
```
**8.** 您可以建立批次程式，以執行**xcopy**作業並使用批次**如果**命令，如果發生錯誤時，程序的結束代碼。 例如，下列批次程式會使用可置換的參數，如**xcopy**來源和目的參數：
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
若要使用上述的批次程式 C:\Prgmcode 目錄及其子目錄中的所有檔案都複製到磁碟機 B，請輸入：
```
copyit c:\prgmcode b:
```
命令直譯器替代**C:\Prgmcode**的 *%1*和**b:** 如 *%2*，然後使用**xcopy**具有 **/e**並 **/s**命令列選項。 如果**xcopy**遇到錯誤，批次程式讀取的結束代碼，並指出在適當的標籤會進入**IF ERRORLEVEL**陳述式，然後顯示適當的訊息，並結束批次的程式。

**9.** 此範例中所有非空白的目錄，再加上檔案名稱符合指定星號符號使用的模式。
```
xcopy .\toc*.yml ..\..\Copy-To\ /S /Y

rem Output example.
rem  .\d1\toc.yml
rem  .\d1\d12\toc.yml
rem  .\d2\toc.yml
rem  3 File(s) copied
```
在上述範例中，這個特定的來源參數值 **。\\toc\*.yml**複製相同的 3 個檔案即使其兩個路徑字元 **。\\**已移除。 不過，沒有檔案會複製如果星號萬用字元已從來源參數，因此只要 **。\\toc.yml**。

#### <a name="BKMK_addref"></a>其他參考資料

-   [[複製]](copy.md)
-   [[移動]](move.md)
-   [Dir](dir.md)
-   [Attrib](attrib.md)
-   [Diskcopy](diskcopy.md)
-   [If](if.md)
-   [命令列語法關鍵](command-line-syntax-key.md)
