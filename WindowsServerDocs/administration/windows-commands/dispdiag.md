---
title: dispdiag
description: Dispdiag 命令的參考文章，其會將顯示資訊記錄到檔案中。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d95a53f60aa7e51450a640d5ec9c5a5e6ccb41a6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931218"
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
