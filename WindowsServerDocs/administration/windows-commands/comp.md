---
title: comp
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 84604cea36b0b4c9543a7169002551c0da4f0493
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379264"
---
# <a name="comp"></a>comp



比較兩個檔案或一組檔案的內容（逐位元組）。 如果使用時不含參數，則**comp**會提示您輸入要比較的檔案。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Data1 >|指定您想要比較的第一個檔案或一組檔案的位置和名稱。 您可以使用萬用字元（ **&#42;** 和 **？** ）來指定多個檔案。|
|\<Data2 >|指定您想要比較的第二個檔案或一組檔案的位置和名稱。 您可以使用萬用字元（ **&#42;** 和 **？** ）來指定多個檔案。|
|/d|以十進位格式顯示差異。 （預設格式為十六進位）。|
|/a|以字元顯示差異。|
|/l|顯示發生差異的行號，而不是顯示位元組位移。|
|/n = \<Number >|只比較針對每個檔案指定的行數，即使檔案的大小不同也一樣。|
|/c|執行不區分大小寫的比較。|
|/off [行]|處理具有離線屬性集的檔案。|
|/?|在命令提示字元顯示 [說明]。|

## <a name="remarks"></a>備註

-   **Comp**命令如何識別不相符的資訊

    在比較期間， **comp**會顯示訊息，以識別檔案之間不相等資訊的位置。 每則訊息都會指出不相等位元組的位移記憶體位址和位元組的內容（除非指定了 **/a**或 **/d**命令列參數，否則為十六進位標記法）。 訊息會以下列格式出現：

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    在十個不相等的比較之後， **comp**會停止比較檔案並顯示下列訊息：

    `10 Mismatches - ending compare`
-   處理*Data1*和*Data2*的特殊案例  
    -   如果您省略*Data1*或*data2*的必要元件，或如果您省略*Data2*，則**comp**會提示您輸入遺漏的資訊。
    -   如果*Data1*只包含磁碟機號或不含檔案名的目錄名稱，則**comp**會比較指定目錄中的所有檔案與*Data1*中指定的檔案。
    -   如果*data2*只包含磁碟機號或目錄名稱，則*data2*的預設檔案名與*Data1*中的名稱相同。
    -   如果 [ **comp** ] 找不到您指定的檔案，它會提示您一則訊息，以判斷是否要比較更多檔案。
-   比較不同位置中的檔案

    **Comp**可以比較相同磁片磁碟機上或不同磁片磁碟機上的檔案，以及相同目錄或不同目錄中的檔案。 當**comp**比較檔案時，它會顯示其位置和檔案名。
-   比較具有相同名稱的檔案

    您比較的檔案可以有相同的檔案名，但前提是它們位於不同的目錄或不同的磁片磁碟機上。 如果您未指定*data2*的檔案名，則*data2*的預設檔案名會與*Data1*中的檔案名相同。 您可以使用萬用字元（ **&#42;** 和 **？** ）來指定檔案名。
-   比較不同大小的檔案

    您必須指定 **/n**來比較不同大小的檔案。 如果檔案大小不同，且未指定 **/n** ，則**comp**會顯示下列訊息：

    `Files are different sizes`

    `Compare more files (Y/N)?`

    若要比較這些檔案，請按 N 以停止 [ **comp** ] 命令。 然後，使用 **/n**選項重新執行**comp**命令，只比較每個檔案的第一個部分。
-   順序比較檔案

    如果您 **&#42;** 使用萬用字元（和 **？** ）來指定多個檔案，則**comp**會尋找符合*Data1*的第一個檔案，並將它與*Data2*中的對應檔案（如果存在的話）進行比較。 **Comp**命令會針對每個符合*Data1*的檔案報告比較結果。 完成時， **comp**會顯示下列訊息：

    `Compare more files (Y/N)?`

    若要比較多個檔案，請按 Y。**Comp**命令會提示您輸入新檔案的位置和名稱。 若要停止比較，請按 N。當您按下 Y 時， **comp**會提示您輸入要使用的命令列選項。 如果您未指定任何命令列選項，則**comp**會使用您先前指定的選項。

## <a name="BKMK_examples"></a>典型

若要比較目錄 C:\Reports 與備份目錄的內容 \\ @ no__t-1Sales\Backup\April，請輸入：
```
comp c:\reports \\sales\backup\april
```
若要比較 \Invoice 目錄中的前10行文字檔，並以十進位格式顯示結果，請輸入：
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)