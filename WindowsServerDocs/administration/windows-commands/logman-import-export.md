---
title: logman 匯入和 logman 匯出
description: Logman 匯入和 logman 匯出的參考主題，可從 XML 檔案匯入資料收集器集合，或將資料收集器集合匯出至 XML 檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44a659abc9e364bf10487e93a7937c1cf8d51bbc
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820568"
---
# <a name="logman-import-and-logman-export"></a>logman 匯入和 logman 匯出

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從 XML 檔案匯入資料收集器集合，或將資料收集器集合匯出至 XML 檔案。

## <a name="syntax"></a>語法

```
logman import <[-n] <name>> <-xml <name>> [options]
logman export <[-n] <name>> <-xml <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -s`<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config`<value>` | 指定包含命令選項的設定檔案。 |
| [-n]`<name>` | 目標物件的名稱。 |
| -xml`<name>` | 要匯入或匯出的 XML 檔案名。 |
| -ets | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
| -[-] u`<user [password]>` | 指定要當做執行身分的使用者。 輸入密碼時，會 `*` 產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
| -y | 對所有問題回答 [是] 而不提示。 |
| /? | 顯示即時線上說明。 |

### <a name="examples"></a>範例

若要從電腦匯入 XML 檔案*c:\windows\ perf_log .xml* *server_1*作為稱為*perf_log*的資料收集器集合，請輸入：

```
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman](logman.md)
