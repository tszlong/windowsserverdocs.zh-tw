---
title: goto
description: Goto 命令的參考文章，會將 cmd.exe 導向至 batch 程式中已標記的行。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afc77f7837ddaeb0552052538537285f0d652682
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924681"
---
# <a name="goto"></a>goto

將 cmd.exe 導向至 batch 程式中的標記行。 在 batch 程式中，此命令會將命令處理導向至標籤所識別的一行。 找到標籤時，處理會繼續從下一行開始的命令開始。

## <a name="syntax"></a>語法

```
goto <label>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<label>` | 指定在 batch 程式中當做標籤使用的文字字串。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

-  如果已啟用命令延伸模組（預設值），而且您使用 [ **goto** ] 命令搭配目標標籤 **： EOF**，則您會將控制權轉移到目前批次腳本檔案的結尾，並結束批次腳本檔案，但不定義標籤。 當您使用此命令搭配 **： EOF**標籤時，您必須在標籤之前插入冒號。 例如：`goto:EOF`。

- 您可以使用*標籤*參數中的空格，但不能包含其他分隔符號（例如，分號（;)或等號（=））。

- 您指定的*標籤*值必須符合 batch 程式中的標籤。 Batch 程式內的標籤必須以冒號（:) 開頭。 如果一行的開頭是冒號，則會將它視為標籤，而該行上的任何命令都會被忽略。 如果您的 batch 程式不包含您在*label*參數中指定的標籤，則 batch 程式會停止，並顯示下列訊息： `Label not found` 。

- 您可以搭配其他命令使用**goto**來執行條件式作業。 如需使用**goto**進行條件式作業的詳細資訊，請參閱[if 命令](if.md)。

## <a name="examples"></a>範例

下列 batch 程式會將磁片磁碟機 A 中的磁片格式化為系統磁片。 如果作業成功， **goto**命令會將處理導向至 **： end**標籤：

```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [cmd 命令](cmd.md)

- [if 命令](if.md)