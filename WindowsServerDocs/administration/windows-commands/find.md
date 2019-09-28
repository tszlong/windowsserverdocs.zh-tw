---
title: find
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 25cd99f3a6411c637a07b7231729cbf529a5d52e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377196"
---
# <a name="find"></a>find



搜尋檔案中的文字字串，並顯示包含指定字串的文字行。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>參數

|           參數           |                                              描述                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    顯示未包含指定之 @no__t 0String > 的所有行。                     |
|              /c               |              計算包含指定之 @no__t 0String > 的行數，並顯示總計。              |
|              /n               |                            在每一行前面加上檔案的行號。                             |
|              /i               |                            指定搜尋不區分大小寫。                            |
|         [/off [行]]          |                        不會略過已設定離線屬性的檔案。                        |
|          「@no__t 0String >」          | 必要。 指定您想要搜尋的字元群組（以引號括住）。 |
| [\<Drive >：][<Path>] <FileName> |        指定要在其中搜尋指定之字串的檔案位置和名稱。        |
|              /?               |                                  在命令提示字元顯示說明。                                  |

## <a name="remarks"></a>備註

-   指定字串

    如果您不使用 **/i**，請**尋找**您針對*字串*指定的確切搜尋。 例如， **find**命令會以不同的方式處理字元 "a" 和 "a"。 不過，如果您使用 **/i**， **find**並不區分大小寫，而且會將 "A" 和 "a" 視為相同的字元。

    如果您想要搜尋的字串包含引號，則必須使用雙引號括住字串中包含的每個引號（例如，"This" 字串 "" contains 引號 "）。
-   使用**尋找**做為篩選準則

    如果您省略檔案名，[**尋找**] 做為篩選準則，接受來自標準輸入來源（通常是鍵盤、管道（|）或重新導向的檔案）的輸入，然後顯示包含*字串*的任何行。
-   排序命令語法

    您可以依任何順序輸入 [**尋找**] 命令的參數和命令列選項。
-   使用萬用字元

    您不能在使用 **&#42;** [**尋找**] 命令所指定的檔案名或副檔名中使用萬用字元（和 **？** ）。 若要搜尋您以萬用字元指定的一組檔案中的字串，您可以在**for**命令中使用 [**尋找**] 命令。
-   使用 **/v**或 **/n**搭配 **/c**

    如果您在相同的命令列中使用 **/c**和 **/v** ，[**尋找**] 會顯示不包含指定字串的行計數。 如果您在相同的命令列中指定 **/c**和 **/n** ， **find**會忽略 **/n**。
-   使用具有回車的**尋找**

    [**尋找**] 命令無法辨識回車。 當您使用 [**尋找**] 來搜尋包含回車的檔案中的文字時，您必須將搜尋字串限制為可在換行字元（也就是不可能因回車符而中斷的字串）之間找到的文字。 例如，如果在「稅額」和「檔案」這兩個字之間出現了換行，則「**尋找**」不會回報字串「稅務檔案」的相符項。

## <a name="BKMK_examples"></a>典型

若要顯示 Pencil.ad 中包含字串 "鉛筆 C-sharpener" 的所有行，請輸入：
```
find "Pencil Sharpener" pencil.ad
```
若要尋找包含引號內文字的字串，您必須以引號括住整個字串。 接著，您必須針對字串中包含的每個引號使用兩個引號。 尋找「已標示其紙張的科學家」，僅供討論之用。」 這不是最後的報告。」 在 [報告] 中，輸入：
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
如果您想要搜尋一組檔案，可以在**for**命令中使用 [**尋找**] 命令。 若要在目前的目錄中搜尋副檔名為 .bat 且包含字串 "PROMPT" 的檔案，請輸入：
```
for %f in (*.bat) do find "PROMPT" %f 
```
若要搜尋硬碟以尋找並顯示磁片磁碟機 C 上包含字串 "CPU" 的檔案名，請使用管線（|）將**dir**命令的輸出導向至 [**尋找**] 命令，如下所示：
```
dir c:\ /s /b | find "CPU" 
```
由於**尋找**搜尋會區分大小寫，而**dir**會產生大寫輸出，因此您必須以大寫字母輸入字串 "CPU"，或使用 **/i**命令列選項搭配**find**。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)