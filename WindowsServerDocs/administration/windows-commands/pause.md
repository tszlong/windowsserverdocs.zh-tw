---
title: pause
description: Pause 命令的參考文章，此命令會暫停批次程式的處理。
ms.topic: reference
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0da314b5b73935fa895b613b827f832492428cc7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032475"
---
# <a name="pause"></a>pause

暫停批次程式的處理，並顯示提示字元 `Press any key to continue . . .`

## <a name="syntax"></a>語法

```
pause
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 如果您按下 CTRL + C 來停止批次程式，則會出現下列訊息： `Terminate batch job (Y/N)?` 。 如果 **您按 (** 下 [是] 的 [是]) 來回應此訊息，批次程式就會結束，控制權會回到作業系統。

- 您可以在可能不想要處理的批次檔區段之前，插入 **pause** 命令。 **暫停**批次程式的處理時，您可以按下 CTRL + C，然後按下**Y**以停止批次程式。

## <a name="examples"></a>範例

若要建立批次程式以提示使用者變更其中一個磁片磁碟機中的磁片，請輸入：

```
@echo off
:Begin
copy a:*.*
echo Put a new disk into Drive A
pause
goto begin
```

在此範例中，磁片磁碟機 A 中磁片上的所有檔案都會複製到目前的目錄。 在訊息提示您將新磁片放入磁片磁碟機 A 之後， **pause** 命令會暫停處理，讓您可以變更磁片，然後按任意鍵繼續處理。 此批次程式會在無限迴圈中執行，而 **goto 開始** 命令會將命令直譯器傳送至批次檔的開始標籤。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)