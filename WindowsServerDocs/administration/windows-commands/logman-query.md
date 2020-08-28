---
title: logman query
description: Logman query 命令的參考文章，可查詢資料收集器或資料收集器集合屬性。
ms.topic: reference
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7d3d9da5b5885c15135b764454bd68818c4aded
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023782"
---
# <a name="logman-query"></a>logman query

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

查詢資料收集器或資料收集器集合的屬性。

## <a name="syntax"></a>語法

```
logman query [providers|Data Collector Set name] [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s `<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config `<value>` | 指定包含命令選項的設定檔。 |
| [-n] `<name>` | 目標物件的名稱。 |
| -ets | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
| /? | 顯示即時線上說明。 |

### <a name="examples"></a>範例

若要列出所有在目標系統上設定的資料收集器集合，請輸入：

```
logman query
```

若要列出包含在名為 *perf_log*的資料收集器集合中的資料收集器，請輸入：

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
