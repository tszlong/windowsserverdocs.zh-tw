---
title: timeout
description: Timeout 的 Windows 命令主題，會在指定的秒數內暫停命令處理器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd0a43e49e8a7567ac975333b04a9e6f549a0fd8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832811"
---
# <a name="timeout"></a>timeout

在指定的秒數內暫停命令處理器。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/t \<TimeoutInSeconds >|指定命令處理器繼續處理之前，要等候的秒數（介於-1 到99999之間）。 值-1 會使電腦無限期地等待按鍵。|
|/nobreak|指定忽略使用者金鑰筆劃。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Timeout**命令通常用於批次檔中。
-   使用者按鍵會立即繼續命令處理器執行，即使超時時間尚未過期也一樣。
-   與**睡眠**命令搭配使用時， **timeout**類似于**pause**命令。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要將命令處理器暫停10秒，請輸入：
```
timeout /t 10
```
若要暫停命令處理器100秒並忽略任何擊鍵，請輸入：
```
timeout /t 100 /nobreak
```
若要無限期地暫停命令處理器直到按下按鍵，請輸入：
```
timeout /t -1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
