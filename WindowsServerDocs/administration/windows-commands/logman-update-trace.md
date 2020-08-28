---
title: logman update trace
description: Logman update trace 命令的參考文章，此命令會更新現有事件追蹤資料收集器的屬性。
ms.topic: reference
ms.assetid: b7111f7f-4162-4d1a-8e53-d766db0ede1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17a63116408458edaf11c2ff44ccf2c1a978cea0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036346"
---
# <a name="logman-update-trace"></a>logman update trace

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

更新現有事件追蹤資料收集器的屬性。

## <a name="syntax"></a>語法

```
logman update trace <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s `<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config `<value>` | 指定包含命令選項的設定檔。 |
| -ets | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
| [-n] `<name>` | 目標物件的名稱。 |
| -f `<bin|bincirc>` | 指定資料收集器的記錄檔格式。 |
| -[-] u `<user [password]>` | 指定要執行的使用者。 輸入 `*` 密碼會產生密碼提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
| -m `<[start] [stop] [[start] [stop] [...]]>` | 手動啟動或停止的變更，而不是排程的開始或結束時間。 |
| -rf `<[[hh:]mm:]ss>` | 在指定的一段時間內執行資料收集器。 |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間開始收集資料。 |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間結束資料收集。 |
| -o `<path|dsn!log>` | 指定 SQL 資料庫中的輸出記錄檔或 DSN 和記錄集名稱。 |
| -[-] r | 每日在指定的開始和結束時間重複資料收集器。 |
| -[-] a | 附加現有的記錄檔。 |
| -[-] 允許 | 覆寫現有的記錄檔。 |
| -[-] v `<nnnnnn|mmddhhmm>` | 將檔案版本設定資訊附加至記錄檔名稱的結尾。 |
| -[-] rc `<task>` | 每次關閉記錄檔時，執行指定的命令。 |
| -[-] max `<value>` | 最大記錄檔大小（以 MB 為單位）或 SQL 記錄檔的最大記錄數目。 |
| -[-] my.cnf `<[[hh:]mm:]ss>` | 當指定時間時，會在經過指定的時間時建立新的檔案。 如果未指定時間，當超過大小上限時，就會建立新的檔案。 |
| -y | 針對所有問題回答「是」，而不提示。 |
| -ct `<perf|system|cycle>` | 指定事件追蹤會話的頻率類型。 |
| -ln `<logger_name>` | 指定事件追蹤會話的記錄器名稱。 |
| -ft `<[[hh:]mm:]ss>` | 指定事件追蹤會話清除計時器。 |
| -[-] p `<provider [flags [level]]>` | 指定要啟用的單一事件追蹤提供者。 |
| -pf `<filename>` | 指定列出多個要啟用之事件追蹤提供者的檔案。 檔案應該是一個文字檔，每行包含一個提供者。 |
| -[-] rt | 以即時模式執行事件追蹤會話。 |
| -[-] ul | 在使用者中執行事件追蹤會話。 |
| -bs `<value>` | 指定事件追蹤會話緩衝區大小（以 kb 為單位）。 |
| -nb `<min max>` | 指定事件追蹤會話緩衝區的數目。 |
| -模式 `<globalsequence|localsequence|pagedmemory>` | 指定事件追蹤會話記錄器模式，包括：<ul><li>**Globalsequence** -指定事件追蹤程式會將序號新增至它所收到的每個事件，而不論哪個追蹤會話收到事件。</li><li>**Localsequence** -指定事件追蹤程式為在特定追蹤會話所收到的事件新增序號。 使用這個選項時，重複的序號可以存在於所有的會話中，但在每個追蹤會話中都是唯一的。</li><li>**Pagedmemory** -指定事件追蹤使用分頁記憶體，而非預設的非分頁式記憶體集區來進行內部緩衝區配置。</li></ul> |
| /? | 顯示即時線上說明。 |

#### <a name="remarks"></a>備註

- 列出 [-] 時，新增額外的連字號 (-) 會否定選項。

### <a name="examples"></a>範例

若要更新名為 *trace_log*的現有事件追蹤資料收集器、將記錄檔大小上限變更為 10 MB、將記錄檔格式更新為 CSV，並以 mmddhhmm 格式附加檔案版本設定，請輸入：

```
logman update trace trace_log -max 10 -f csv -v mmddhhmm
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman create trace 命令](logman-create-trace.md)

- [logman 命令](logman.md)
