---
title: find
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4639ff780687ad7a69ddba5374a722a15b06542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848259"
---
# <a name="find"></a>find



搜尋的檔案或檔案中的文字字串，並顯示的文字行，其中包含指定的字串。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/v|會顯示不包含指定的所有行\<字串 >。|
|/c|計算包含指定的行\<字串 >，並顯示總計。|
|/n|在檔案的行號的每一行的前面。|
|/i|指定搜尋不區分大小寫。|
|[/off[line]]|不會略過具有離線的屬性集的檔案。|
|「\<字串 >"|必要。 指定您想要搜尋的字元 （加上引號） 的群組。|
|[\<Drive>:][<Path>]<FileName>|指定要在其中搜尋指定之字串的檔案名稱與位置。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   指定的字串

    如果您不要使用 **/i**，**尋找**完全您所指定的搜尋*字串*。 例如，**尋找**命令會將這些字元"a"和"A"以不同的方式。 如果您使用 **/i**，不過**尋找**不區分大小寫，而且它會將"a"和"A"做為相同的字元。

    如果您想要搜尋的字串包含引號，您必須使用雙引號的字串內所包含每個引號 （例如，"This""string""包含引號"）。
-   使用**尋找**做為篩選條件

    如果您省略檔案名稱，**尋找**做為篩選條件，從標準輸入來源 （通常是鍵盤，垂直線 (|) 或重新導向的檔案） 取得輸入，並顯示任何行，其中包含*字串*。
-   排序的命令語法

    您可以輸入參數和命令列選項**尋找**命令依任何順序。
-   使用萬用字元

    您不能使用萬用字元 (**&#42;** 並 **？**) 中的檔案名稱或您使用指定的延伸模組**尋找**命令。 若要搜尋一組您指定具有萬用字元的檔案中的字串，您可以使用**尋找**命令內**如**命令。
-   使用 **/v**或是 **/n**使用 **/c**

    如果您使用 **/c**並 **/v**相同的命令列中**尋找**顯示並包含指定的字串行計數。 如果您指定 **/c**並 **/n**相同的命令列中**尋找**忽略 **/n**。
-   使用**尋找**換行字元會傳回

    **尋找**命令無法辨識換行字元。 當您使用**尋找**搜尋文字包含歸位字元的檔案中，您必須限制可換行字元 （也就是不太可能會暫時中斷歸位字元字串） 之間的文字搜尋字串。 例如，**尋找**歸位字元，就會發生之間的單字"稅務"和"file"。 不會報告與字串"稅務檔案 」

## <a name="BKMK_examples"></a>範例

若要顯示從 Pencil.ad 包含字串"削鉛筆機 」 的所有行，請輸入：
```
find "Pencil Sharpener" pencil.ad
```
若要尋找包含在引號內的文字字串，您必須將整個字串括在引號中。 然後您必須將兩個引號用於字串內所包含的各引號。 若要尋找 「 科學家標記其文件 」 僅限於討論。 」 它不是最終報表。 」 影響 report.doc，請輸入：
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
如果您想要搜尋的一組檔案，您可以使用**尋找**命令內**如**命令。 若要搜尋目前目錄之檔案的副檔名為.bat，以及包含字串"PROMPT"，類型：
```
for %f in (*.bat) do find "PROMPT" %f 
```
若要搜尋來尋找及顯示檔案名稱包含字串"CPU"的磁碟機 C 上的硬碟，請使用直立線符號 (|) 的輸出**dir**命令以**尋找**命令，如下所示：
```
dir c:\ /s /b | find "CPU" 
```
因為**尋找**搜尋會區分大小寫並**dir**會產生大寫字母的輸出，您必須以大寫字母輸入"CPU"的字串，或使用 **/i**命令列選項與**尋找**。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)