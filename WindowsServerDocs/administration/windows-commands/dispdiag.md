---
title: dispdiag
description: Dispdiag 命令的參考文章，記錄檔中顯示資訊。
ms.topic: reference
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d2171aae18601f3783389335ea1cfa592a5c7f23
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627152"
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
