---
title: pause
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 135d074a71c7153cc1665ad7b543bdba56ed66e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837681"
---
# <a name="pause"></a>pause



暫停 batch 程式的處理，並顯示下列提示：
```
Press any key to continue . . .
```
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
pause
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- 當您執行**pause**命令時，會出現下列訊息：  
  ```
  Press any key to continue . . .
  ```  
- 如果您按下 CTRL + C 來停止 batch 程式，則會出現下列訊息：  
  ```
  Terminate batch job (Y/N)?
  ```  
  如果您按 Y （為 [是]）來回應此訊息，則 batch 程式會結束，且控制權會回到作業系統。
- 您可以在可能不想要處理的批次檔區段之前，插入**pause**命令。 **暫停**batch 程式的處理時，您可以按下 CTRL + C，然後按 Y 以停止 batch 程式。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要建立批次程式，以提示使用者變更其中一個磁片磁碟機中的磁片，請輸入：
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
在此範例中，磁片磁碟機 A 中磁片上的所有檔案都會複製到目前的目錄。 當訊息提示您將新磁片放在磁片磁碟機 A 之後， **pause**命令就會暫停處理，讓您可以變更磁片，然後按任意鍵繼續處理。 此批次程式會在無止盡的迴圈中執行， **goto begin**命令會將命令直譯器傳送至批次檔的開始標籤。 若要停止此 batch 程式，請按 CTRL + C，然後按下 Y 鍵。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)