---
title: logman import and logman export
description: Logman 匯入和 logman 匯出的參考文章，可從 XML 檔案匯入資料收集器集合，或將資料收集器集合器匯出至 XML 檔案。
ms.topic: reference
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fb750b2ba514c28b05b7b7817994aef3b83eb62
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023792"
---
# <a name="logman-import-and-logman-export"></a>logman import and logman export

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從 XML 檔案匯入資料收集器集合，或將資料收集器集合器匯出至 XML 檔案。

## <a name="syntax"></a>語法

```
logman import <[-n] <name> <-xml <name> [options]
logman export <[-n] <name> <-xml <name> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s `<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config `<value>` | 指定包含命令選項的設定檔。 |
| [-n] `<name>` | 目標物件的名稱。 |
| -xml `<name>` | 要匯入或匯出的 XML 檔案名。 |
| -ets | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
| -[-] u `<user [password]>` | 指定要執行的使用者。 輸入 `*` 密碼會產生密碼提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
| -y | 針對所有問題回答「是」，而不提示。 |
| /? | 顯示即時線上說明。 |

### <a name="examples"></a>範例

若要從電腦*server_1*將 XML 檔案*c:\windows\perf_log.xml*匯入為資料收集器集合人員（稱為*perf_log*），請輸入：

```
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 命令](logman.md)
