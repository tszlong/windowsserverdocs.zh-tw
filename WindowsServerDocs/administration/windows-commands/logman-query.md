---
title: logman query
description: Logman 查詢命令的參考文章，它會查詢資料收集器或資料收集器集合屬性。
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2cb324651001f071e45acf0821f402458ed838d8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887279"
---
# <a name="logman-query"></a>logman query

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

查詢資料收集器或資料收集器集合屬性。

## <a name="syntax"></a>語法

```
logman query [providers|Data Collector Set name] [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s`<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config`<value>` | 指定包含命令選項的設定檔案。 |
| [-n]`<name>` | 目標物件的名稱。 |
| -ets | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
| /? | 顯示即時線上說明。 |

### <a name="examples"></a>範例

若要列出目標系統上設定的所有資料收集器集合，請輸入：

```
logman query
```

若要列出包含在名為*perf_log*之資料收集器集合中的資料收集器，請輸入：

```
logman query perf_log
```

若要列出目標系統上所有可用的資料收集器提供者，請輸入：

```
logman query providers
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)
