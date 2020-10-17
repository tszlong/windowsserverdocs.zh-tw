---
title: timeout
description: Timeout 命令的參考文章，會在指定的秒數後暫停命令處理器。
ms.topic: reference
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: edb4d14614099b0e501df175e619285928344c78
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156673"
---
# <a name="timeout"></a>timeout

在指定的秒數後暫停命令處理器。 此命令通常用於批次檔中。

## <a name="syntax"></a>語法

```
timeout /t <timeoutinseconds> [/nobreak]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 一起 `<timeoutinseconds>` | 指定介於-1 和99999之間的小數秒數 (，) 在命令處理器繼續處理之前等候。 值 **-1** 會使電腦無限期地等候按鍵。 |
| /nobreak | 指定忽略使用者按鍵筆劃。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 使用者按鍵會立即繼續執行命令處理器，即使超時期間尚未過期也是一樣。

- 與資源套件的 **睡眠** 工具搭配使用時， **timeout** 類似于 **pause** 命令。

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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
