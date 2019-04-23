---
title: comp
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d10b86176d97e1afd76085516fbfc00bdc36577f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854679"
---
# <a name="comp"></a>comp



比較兩個檔案的內容或檔案的位元組集合。 如果未指定參數，使用**comp**會提示您輸入檔案進行比較。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Data1 >|指定的位置和名稱的第一個檔案或一組您想要比較的檔案。 您可以使用萬用字元 (**&#42;** 並 **？**) 來指定多個檔案。|
|\<Data2>|指定的位置和名稱的第二個檔案或一組您想要比較的檔案。 您可以使用萬用字元 (**&#42;** 並 **？**) 來指定多個檔案。|
|/d|顯示十進位格式的差異。 （預設的格式是十六進位）。|
|/a|顯示為字元的差異。|
|/l|顯示差異的發生位置，而不是顯示位元組位移的行號。|
|/n=\<Number>|即使檔案是不同的大小，請比較只針對每個檔案，所指定的行數。|
|/c|執行不區分大小寫的比較。|
|/off[line]|處理檔案，並且會設定離線屬性。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   如何**comp**命令會找出不相符的資訊

    相較之下，期間**comp**會顯示訊息，找出之檔案之間的不相等資訊的位置。 每則訊息表示的相等的位元組位移的記憶體位址和內容的位元組 (以十六進位標記法除非 **/a**或是 **/d**指定命令列參數)。 以下列格式，會出現訊息：

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    十個不相等比較之後, **comp**停止比較檔案，並顯示下列訊息：

    `10 Mismatches - ending compare`
-   處理特殊的情況下，如*Data1*和*Data2*  
    -   如果您省略的必要元件*Data1*或是*Data2*或如果您省略*Data2*， **comp**會提示您輸入遺漏的資訊。
    -   如果*Data1*包含磁碟機代號或使用任何檔案名稱、 目錄名稱**comp**比較中的所有檔案中指定的檔案指定的目錄*Data1*。
    -   如果*Data2*包含磁碟機代號或目錄名稱的預設檔案名稱*Data2* ，在相同*Data1*。
    -   如果**comp**找不到指定，它會提示您使用的訊息，以判斷您是否想要比較多個檔案的檔案。
-   比較在不同位置的檔案

    **Comp**可以比較檔案相同的磁碟機上或在不同的磁碟機，和相同的目錄中或在不同的目錄。 當**comp**比較的檔案，它會顯示其位置和檔案名稱。
-   比較具有相同名稱的檔案

    您比較的檔案可以有相同的檔案名稱，前提是它們在不同的目錄，或在不同的磁碟機上。 如果您未指定的檔名*Data2*，預設檔案名稱*Data2*中的檔案名稱與相同*Data1*。 您可以使用萬用字元 (**&#42;** 並 **？**) 來指定檔案名稱。
-   比較不同大小的檔案

    您必須指定 **/n**來比較不同大小的檔案。 如果檔案大小都不同， **/n**未指定，則**comp**顯示下列訊息：

    `Files are different sizes`

    `Compare more files (Y/N)?`

    要比較這些檔案，請按下 N 來停止**comp**命令。 然後，重新執行**comp**命令搭配 **/n**来比較的第一個部分的每個檔案的選項。
-   依序比較檔案

    如果您使用萬用字元 (**&#42;** 並 **？**) 來指定多個檔案**comp**尋找第一個符合的檔案*Data1*並比較它與對應的檔案中*Data2*，如果有的話。 **Comp**命令會報告每個檔案符合的比較結果*Data1*。 完成時， **comp**顯示下列訊息：

    `Compare more files (Y/N)?`

    若要比較多個檔案，按下 Y。**Comp**命令會提示您輸入的位置和新的檔案名稱。 若要停止的比較，按 n。當您按下 Y **comp**會提示您輸入要使用的命令列選項。 如果您未指定任何命令列選項， **comp**會使用您先前指定的項目。

## <a name="BKMK_examples"></a>範例

要比較的內容具有備份目錄的目錄 C:\Reports \\ \\Sales\Backup\April，型別：
```
comp c:\reports \\sales\backup\april
```
若要比較 \Invoice 目錄中的文字檔案的前十個幾行，而以十進位格式顯示結果，請輸入：
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)