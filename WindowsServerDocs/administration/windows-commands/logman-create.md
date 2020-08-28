---
title: logman create
description: Logman create 命令的參考文章，可建立計數器、追蹤、設定資料收集器或 API。
ms.topic: reference
ms.assetid: 972f0126-7bc4-4b14-9265-062864f3ffd4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8635bfef8e9a82175348bdc06b5b722c8a1e733d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023812"
---
# <a name="logman-create"></a>logman create

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立計數器、追蹤、設定資料收集器或 API。

## <a name="syntax"></a>語法

```
logman create <counter | trace | alert | cfg | api> <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [logman create counter](logman-create-counter.md) | 建立計數器資料收集器。 |
| [logman create trace](logman-create-trace.md) | 建立追蹤資料收集器。 |
| [logman create alert](logman-create-alert.md) | 建立警示資料收集器。 |
| [logman create cfg](logman-create-cfg.md) | 建立設定資料收集器。 |
| [logman create api](logman-create-api.md) | 建立 API 追蹤資料收集器。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)