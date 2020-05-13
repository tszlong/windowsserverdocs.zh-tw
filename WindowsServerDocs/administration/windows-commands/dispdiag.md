---
title: dispdiag
description: Dispdiag 命令的參考主題，其會將顯示資訊記錄到檔案中。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff4e3690ec3b2c9d473f05027d5637eda124d0ba
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235224"
---
# <a name="dispdiag"></a>dispdiag

記錄會將資訊顯示到檔案。

## <a name="syntax"></a>語法

```
dispdiag [-testacpi] [-d] [-delay <seconds>] [-out <filepath>]
```

#### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| - testacpi | 執行熱鍵診斷測試。 顯示測試期間按下之任何金鑰的金鑰名稱、程式碼和掃描代碼。 |
| -d | 產生包含測試結果的傾印檔案。 |
| -延遲`<seconds>` | 依指定的時間 *（以秒為單位）* 延遲資料收集。 |
| -out`<filepath>`  | 指定路徑和檔案名，以儲存收集的資料。 這必須是最後一個參數。 |
| -? | 顯示可用的命令參數，並提供使用它們的協助。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
