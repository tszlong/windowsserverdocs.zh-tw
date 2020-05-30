---
title: logman create
description: Logman create 命令的參考主題，它會建立計數器、追蹤、設定資料收集器或 API。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 972f0126-7bc4-4b14-9265-062864f3ffd4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4a68be098f868cdd9cd48c1e7c68fc183fa1fab
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222957"
---
# <a name="logman-create"></a>logman create

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立計數器、追蹤、設定資料收集器或 API。

## <a name="syntax"></a>語法

```
logman create <counter | trace | alert | cfg | api> <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [logman 建立計數器](logman-create-counter.md) | 建立計數器資料收集器。 |
| [logman 建立追蹤](logman-create-trace.md) | 建立追蹤資料收集器。 |
| [logman 建立警示](logman-create-alert.md) | 建立警示資料收集器。 |
| [logman 建立 cfg](logman-create-cfg.md) | 建立設定資料收集器。 |
| [logman 建立 api](logman-create-api.md) | 建立 API 追蹤資料收集器。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)