---
title: logman
description: Logman 命令的參考主題，它會建立和管理事件追蹤會話和效能記錄檔，並從命令列支援效能監視器的許多功能。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44b5e134440d71eed61ca8e03739abcc962df1f9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820548"
---
# <a name="logman"></a>logman

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立和管理事件追蹤工作階段和效能記錄檔，並支援從命令列執行效能監視器的許多功能。

## <a name="syntax"></a>語法

```
logman [create | query | start | stop | delete| update | import | export | /?] [options]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| [logman create](logman-create.md) | 建立計數器、追蹤、設定資料收集器或 API。 |
| [logman query](logman-query.md) | 查詢資料收集器屬性。 |
| [logman start &#124; stop](logman-start-stop.md) | 開始或停止資料收集。 |
| [logman delete](logman-delete.md) | 刪除現有的資料收集器。 |
| [logman update](logman-update.md) | 更新現有資料收集器的屬性。 |
| [logman import &#124; export](logman-import-export.md) | 從 XML 檔案匯入資料收集器集合檔，或將資料收集器集合匯出至 XML 檔案。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)