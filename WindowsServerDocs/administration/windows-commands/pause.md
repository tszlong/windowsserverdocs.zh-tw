---
title: pause
description: Pause 命令的參考文章，會暫停批次程式的處理。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f604bbd205a074d8966cd2c1a1bc65506e7ca5e0
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922897"
---
# <a name="pause"></a>pause

暫停處理 batch 程式，並顯示提示字元`Press any key to continue . . .`

## <a name="syntax"></a>語法

```
pause
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- 如果您按下 CTRL + C 來停止 batch 程式，則會出現下列訊息： `Terminate batch job (Y/N)?` 。 如果您按**Y** （為 [是]）來回應此訊息，則 batch 程式會結束，且控制權會回到作業系統。

- 您可以在可能不想要處理的批次檔區段之前，插入**pause**命令。 **暫停**batch 程式的處理時，您可以按下 CTRL + C，然後按**Y**以停止 batch 程式。

## <a name="examples"></a>範例

若要建立批次程式，以提示使用者變更其中一個磁片磁碟機中的磁片，請輸入：

```
@echo off
:Begin
copy a:*.*
echo Put a new disk into Drive A
pause
goto begin
```

在此範例中，磁片磁碟機 A 中磁片上的所有檔案都會複製到目前的目錄。 當訊息提示您將新磁片放在磁片磁碟機 A 之後， **pause**命令就會暫停處理，讓您可以變更磁片，然後按任意鍵繼續處理。 此批次程式會在無止盡的迴圈中執行， **goto begin**命令會將命令直譯器傳送至批次檔的開始標籤。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)