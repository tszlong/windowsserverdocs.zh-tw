---
title: timeout
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3997399b732c494050797c83a0a52938574986bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830169"
---
# <a name="timeout"></a>timeout



指定的秒數就會暫停命令處理器。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/t \<TimeoutInSeconds>|指定命令處理器就會繼續處理之前要等候的秒數 （介於-1 到 99999） 的十進位數字。 值-1 會導致無限期地等待按鍵動作的電腦。|
|/nobreak|指定忽略使用者按鍵。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **逾時**命令通常會在批次檔。
-   立即執行，即使尚未過期的逾時期限時，使用者按鍵輸入將繼續命令處理器執行。
-   當搭配**睡眠**命令，**逾時**類似**暫停**命令。

## <a name="BKMK_examples"></a>範例

若要每十秒暫停命令處理器，請輸入：
```
timeout /t 10
```
若要暫停 100 秒命令處理器，並忽略任何按鍵輸入，輸入：
```
timeout /t 100 /nobreak
```
若要暫停命令處理，直到按下按鍵，請輸入：
```
timeout /t -1
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
