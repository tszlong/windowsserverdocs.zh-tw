---
title: logman update
description: Logman update 命令的參考文章，此命令會更新現有的資料收集器。
ms.topic: reference
ms.assetid: c98af84f-64ba-40c3-826d-75b80dfb9b34
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 561c386b8bed962a569b7f75b655980d3c84da84
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037296"
---
# <a name="logman-update"></a>logman update

更新現有的資料收集器。

## <a name="syntax"></a>語法

```
logman update <counter | trace | alert | cfg | api> <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------| ----------- |
| [logman update counter](logman-update-counter.md) | 更新計數器資料收集器。 |
| [logman update alert](logman-update-alert.md) | 更新警示資料收集器。 |
| [logman update cfg](logman-update-cfg.md) | 更新設定資料收集器。 |
| [logman update api](logman-update-api.md) | 更新 API 追蹤資料收集器。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)
