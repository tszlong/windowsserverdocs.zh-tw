---
title: logman create
description: Logman create 命令的參考文章，它會建立計數器、追蹤、設定資料收集器或 API。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 972f0126-7bc4-4b14-9265-062864f3ffd4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 695a101a0aa6a720b64ffee6617085d13b6e83d1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922328"
---
# <a name="logman-create"></a>logman create

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立計數器、追蹤、設定資料收集器或 API。

## <a name="syntax"></a>語法

```
logman create <counter | trace | alert | cfg | api> <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| [logman create counter](logman-create-counter.md) | 建立計數器資料收集器。 |
| [logman create trace](logman-create-trace.md) | 建立追蹤資料收集器。 |
| [logman create alert](logman-create-alert.md) | 建立警示資料收集器。 |
| [logman create cfg](logman-create-cfg.md) | 建立設定資料收集器。 |
| [logman create api](logman-create-api.md) | 建立 API 追蹤資料收集器。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)