---
title: timeout
description: Timeout 的參考文章，會在指定的秒數後暫停命令處理器。
ms.topic: reference
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4905eaadc745fc5499cb393b1808794e2f803361
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038256"
---
# <a name="timeout"></a>timeout

在指定的秒數後暫停命令處理器。



## <a name="syntax"></a>語法

```
timeout /t <TimeoutInSeconds> [/nobreak]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|一起 \<TimeoutInSeconds>|指定介於-1 和99999之間的小數秒數 (，) 在命令處理器繼續處理之前等候。 值-1 會使電腦無限期地等候按鍵。|
|/nobreak|指定忽略使用者按鍵筆劃。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Timeout**命令通常用於批次檔中。
-   使用者按鍵會立即繼續執行命令處理器，即使超時期間尚未過期也是一樣。
-   搭配 **sleep** 命令使用時， **timeout** 類似于 **pause** 命令。

## <a name="examples"></a>範例

若要暫停命令處理器10秒，請輸入：
```
timeout /t 10
```
若要將命令處理器暫停100秒，並忽略任何按鍵，請輸入：
```
timeout /t 100 /nobreak
```
若要無限期地暫停命令處理器，直到按下按鍵為止，請輸入：
```
timeout /t -1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
