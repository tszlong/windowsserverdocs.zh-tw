---
title: 尋找
description: Find 命令的參考文章，會搜尋檔案中的文字字串，並在檔案中顯示指定的文字字串。
ms.topic: reference
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b0317e856153c014df2656f0a98452b0c6428e82
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622375"
---
# <a name="find"></a>尋找

搜尋檔案或檔案中的文字字串，並顯示包含指定字串的文字行。

## <a name="syntax"></a>語法

```
find [/v] [/c] [/n] [/i] [/off[line]] <string> [[<drive>:][<path>]<filename>[...]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /v | 顯示不包含指定之的所有行 `<string>` 。 |
| /C | 計算包含指定之的行， `<string>` 並顯示總計。 |
| /n | 在每一行前面加上檔案的行號。 |
| /i | 指定搜尋不區分大小寫。 |
| [/off [行]] | 不會略過已設定 offline 屬性的檔案。 |
| `<string>` | 必要。 指定要搜尋的字元群組 (以引號括住) 。 |
| `[<drive>:][<path>]<filename>` | 指定要在其中搜尋指定字串之檔案的位置和名稱。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果您不使用 **/i**，此命令會搜尋您指定的 *字串*內容。 例如，此命令會以不同的方式來處理字元 `a` 和 `A` 。 但是，如果您使用 **/i**，搜尋就會變成不區分大小寫，而且會將 `a` 和視為 `A` 相同的字元。

- 如果您想要搜尋的字串包含引號，則必須針對字串中包含的每個引號使用雙引號 (例如 "" This string contains 引號 "" ) 。

- 如果您省略檔案名，此命令會作為篩選器，接受標準輸入來源的輸入 (通常是鍵盤、管道 (|) 或重新導向的檔案) 然後顯示任何包含 *字串*的行。

- 您可以依任何順序輸入 [ **尋找** ] 命令的參數和命令列選項。

- 您無法在使用此命令時指定的檔案名或副檔名中使用萬用字元 (**&#42;** 和 **？**) 。 若要在使用萬用字元指定的一組檔案中搜尋字串，您可以在 **for** 命令中使用這個命令。

- 如果您在相同的命令列中使用 **/c** 和 **/v** ，此命令會顯示不包含指定字串的行計數。 如果您在相同的命令列中指定 **/c** 和 **/n** ， **find** 會忽略 **/n**。

- 此命令無法辨識傳回的回車。 當您使用此命令來搜尋包含換行字元之檔案中的文字時，您必須將搜尋字串限制為可在換行字元之間找到的文字 (也就是不太可能被換行傳回) 的字串。 例如，如果單字和檔案之間有換行，此命令就不會回報字串稅務檔的相符項。

### <a name="examples"></a>範例

若要顯示 *pencil.ad* 中包含字串 *鉛筆 c-sharpener*的所有行，請輸入：

```
find pencil sharpener pencil.ad
```

若要尋找文字，「科學家只標示了他們的書面檔以進行討論。 這不是最終報告。」 在 *report.doc* 檔案中，輸入：

```
find ""The scientists labeled their paper for discussion only. It is not a final report."" report.doc
```

若要搜尋一組檔案，您可以使用**for**命令中的 [**尋找**] 命令。 若要在目前的目錄中搜尋副檔名為 .bat 且包含字串提示的檔案，請輸入：

```
for %f in (*.bat) do find PROMPT %f
```

若要搜尋硬碟以尋找並顯示包含字串 CPU 的 C 磁片磁碟機上的檔案名，請使用管道 (|) 將 **dir** 命令的輸出導向 [ **尋找** ] 命令，如下所示：

```
dir c:\ /s /b | find CPU
```

因為**尋找**搜尋會區分大小寫，而**dir**會產生大寫輸出，所以您必須以大寫字母輸入字串 CPU，或搭配**find**使用 **/i**命令列選項。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [for 命令](for.md)