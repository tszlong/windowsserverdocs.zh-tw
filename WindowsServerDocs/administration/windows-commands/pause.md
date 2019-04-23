---
title: pause
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 109d162e8d5c4bdd59871a21f16b6f568df4fbd6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861659"
---
# <a name="pause"></a>pause



會暫停處理的批次程式，並顯示下列提示：
```
Press any key to continue . . .
```
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
pause
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   當您執行**暫停**命令時，會出現下列訊息：  
    ```
    Press any key to continue . . .
    ```  
-   如果您按下 CTRL + C 來停止批次程式，就會出現下列訊息：  
    ```
    Terminate batch job (Y/N)?
    ```  
    如果您按 Y (是），以回應這個訊息，批次程式便會結束並將控制傳回至作業系統。
-   您可以插入**暫停**命令之前，您可能不想要處理的批次檔的區段。 當**暫停**暫止處理的批次程式，您可以按 CTRL + C，再按 停止批次程式 Y。

## <a name="BKMK_examples"></a>範例

若要建立批次程式，以提示使用者變更其中一種磁碟機的磁碟，請輸入：
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
在此範例中，在磁碟機 a 的磁碟上的所有檔案都複製到目前的目錄。 訊息會提示您將新的磁碟放在磁碟機之後,**暫停**命令會暫停處理，讓您可以變更磁碟，然後按下任意鍵以繼續處理。 批次執行此程式在無止盡的迴圈 — **goto 開始**命令會將命令解譯器傳送至批次檔的開始標籤。 若要停止此批次程式，請按 CTRL + C，，然後按 Y。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)