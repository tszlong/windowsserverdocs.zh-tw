---
title: dispdiag
description: Dispdiag 命令的參考文章，記錄檔中顯示資訊。
ms.topic: reference
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92dd0b49d8907f3ec934fd59d61b0504b622e80b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030836"
---
# <a name="dispdiag"></a>dispdiag

記錄檔中顯示資訊。

## <a name="syntax"></a>語法

```
dispdiag [-testacpi] [-d] [-delay <seconds>] [-out <filepath>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| - testacpi | 執行熱鍵診斷測試。 針對測試期間所按下的任何按鍵，顯示金鑰名稱、程式碼和掃描程式碼。 |
| -d | 產生含有測試結果的傾印檔案。 |
| -延遲 `<seconds>` | 依指定的時間 *（以秒為單位）* 延遲收集資料。 |
| -out `<filepath>`  | 指定路徑和檔案名以儲存收集的資料。 這必須是最後一個參數。 |
| -? | 顯示可用的命令參數，並提供使用這些參數的說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
