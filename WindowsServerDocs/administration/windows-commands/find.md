---
title: 尋找
description: Find 命令的參考主題，它會搜尋檔案中的文字字串，並在檔案中顯示指定的文字字串。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0273405ce5e5b4958a347cd1eaddee0a38897f0c
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437003"
---
# <a name="find"></a>尋找

搜尋檔案中的文字字串，並顯示包含指定字串的文字行。

## <a name="syntax"></a>語法

```
find [/v] [/c] [/n] [/i] [/off[line]] <string> [[<drive>:][<path>]<filename>[...]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /v | 顯示不包含指定之的所有行 `<string>` 。 |
| /C | 計算包含指定之的行數 `<string>` ，並顯示總計。 |
| /n | 在每一行前面加上檔案的行號。 |
| /i | 指定搜尋不區分大小寫。 |
| [/off [行]] | 不會略過已設定離線屬性的檔案。 |
| `<string>` | 必要。 指定您想要搜尋的字元群組（以引號括住）。 |
| `[<drive>:][<path>]<filename>` | 指定要在其中搜尋指定之字串的檔案位置和名稱。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您不使用 **/i**，此命令會搜尋您針對*string*所指定的確切內容。 例如，此命令會以不同的方式來處理字元 `a` `A` 。 不過，如果您使用 **/i**，搜尋就會變成不區分大小寫，而且會將 `a` 和視為 `A` 相同的字元。

- 如果您想要搜尋的字串包含引號，則必須使用雙引號括住字串中包含的每個引號（例如 ""，此字串包含引號 ""）。

- 如果您省略檔案名，此命令會作為篩選準則，接受來自標準輸入來源（通常是鍵盤、管道（|）或重新導向的檔案）的輸入，然後顯示包含*字串*的任何行。

- 您可以依任何順序輸入 [**尋找**] 命令的參數和命令列選項。

- 使用此命令時，您不能在所指定的檔案名或副檔名中使用萬用字元（**&#42;** 和 **？**）。 若要搜尋您以萬用字元指定的一組檔案中的字串，您可以在**for**命令中使用此命令。

- 如果您在相同的命令列中使用 **/c**和 **/v** ，此命令會顯示不包含指定字串的行計數。 如果您在相同的命令列中指定 **/c**和 **/n** ， **find**會忽略 **/n**。

- 此命令無法辨識回車。 當您使用此命令來搜尋包含回車的檔案中的文字時，您必須將搜尋字串限制為可在換行字元之間找到的文字（也就是不太可能會中斷的字串）。 例如，如果在稅務和 file 這兩個字之間發生了回車，此命令就不會報告字串稅務檔案的相符結果。

### <a name="examples"></a>範例

若要顯示*pencil.ad*中包含字串*鉛筆 c-sharpener*的所有行，請輸入：

```
find pencil sharpener pencil.ad
```

若要尋找文字，「科學家將其論文標示為僅供討論。 這不是最後的報告。」 在*報表 .doc*檔案中，輸入：

```
find ""The scientists labeled their paper for discussion only. It is not a final report."" report.doc
```

若要搜尋一組檔案，您可以在**for**命令中使用 [**尋找**] 命令。 若要在目前的目錄中搜尋副檔名為 .bat 且包含字串提示的檔案，請輸入：

```
for %f in (*.bat) do find PROMPT %f
```

若要搜尋硬碟以尋找並顯示磁片磁碟機 C 上包含字串 CPU 的檔案名，請使用管線（|）將**dir**命令的輸出導向至 [**尋找**] 命令，如下所示：

```
dir c:\ /s /b | find CPU
```

由於**尋找**搜尋會區分大小寫，而**dir**會產生大寫輸出，因此您必須以大寫字母輸入字串 CPU，或使用 **/i**命令列選項搭配**find**。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [針對命令](for.md)