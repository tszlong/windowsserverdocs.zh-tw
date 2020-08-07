---
title: dispdiag
description: Dispdiag 命令的參考文章，其會將顯示資訊記錄到檔案中。
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78b8080395fa30a934d1a9380bf86beae7e62dc0
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890821"
---
# <a name="dispdiag"></a>dispdiag

記錄會將資訊顯示到檔案。

## <a name="syntax"></a>語法

```
dispdiag [-testacpi] [-d] [-delay <seconds>] [-out <filepath>]
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| - testacpi | 執行熱鍵診斷測試。 顯示測試期間按下之任何金鑰的金鑰名稱、程式碼和掃描代碼。 |
| -d | 產生包含測試結果的傾印檔案。 |
| -延遲`<seconds>` | 依指定的時間 *（以秒為單位）* 延遲資料收集。 |
| -out`<filepath>`  | 指定路徑和檔案名，以儲存收集的資料。 這必須是最後一個參數。 |
| -? | 顯示可用的命令參數，並提供使用它們的協助。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
