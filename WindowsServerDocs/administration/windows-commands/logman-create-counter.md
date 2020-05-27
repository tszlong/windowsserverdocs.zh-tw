---
title: logman 建立計數器
description: Logman create counter 命令的參考主題，它會建立計數器資料收集器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e214c32-b704-43c1-b548-e1cf43b583c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60700190146a3a8b84e1121f01d9168196dad888
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820438"
---
# <a name="logman-create-counter"></a>logman 建立計數器

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立計數器資料收集器。

## <a name="syntax"></a>語法

```
logman create counter <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -s`<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config`<value>` | 指定包含命令選項的設定檔案。 |
| [-n]`<name>` | 目標物件的名稱。 |
| -f`<bin|bincirc>` | 指定資料收集器的記錄格式。 |
| -[-] u`<user [password]>` | 指定要當做執行身分的使用者。 輸入密碼時，會 `*` 產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
| -m`<[start] [stop] [[start] [stop] [...]]>` | 手動啟動或停止的變更，而不是排程的開始或結束時間。 |
| -rf`<[[hh:]mm:]ss>` | 在指定的時間內執行資料收集器。 |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間開始收集資料。 |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間結束資料收集。 |
| -si`<[[hh:]mm:]ss>` | 指定效能計數器資料收集器的取樣間隔。 |
| -o`<path|dsn!log>` | 指定 SQL 資料庫中的輸出記錄檔或 DSN 和記錄集名稱。 |
| -[-] r | 每日在指定的開始和結束時間重複資料收集器。 |
| -[-] a | 附加現有的記錄檔。 |
| -[-] 允許 | 覆寫現有的記錄檔。 |
| -[-] v`<nnnnnn|mmddhhmm>` | 將檔案版本設定資訊附加至記錄檔名稱的結尾。 |
| -[-] rc`<task>` | 每次關閉記錄檔時，執行指定的命令。 |
| -[-] max`<value>` | 最大記錄檔大小（MB）或 SQL 記錄檔的最大記錄數目。 |
| -[-] my.cnf`<[[hh:]mm:]ss>` | 當指定時間時，請在指定的時間已過時建立新的檔案。 未指定時間時，請在超過最大大小時建立新的檔案。 |
| -y | 對所有問題回答 [是] 而不提示。 |
| -cf`<filename>` | 指定要收集的效能計數器清單。 檔案每行應該包含一個效能計數器名稱。 |
| -c`<path [path [ ]]>` | 指定要收集的效能計數器。 |
| -sc`<value>` | 指定要使用效能計數器資料收集器收集的最大樣本數。 |
| /? | 顯示即時線上說明。 |

#### <a name="remarks"></a>備註

- 其中列出 [-]，加入額外的連字號（-）會否定選項。

### <a name="examples"></a>範例

若要使用 [處理器（_Total）] 計數器類別中的 [% Processor time] 計數器建立名為*perf_log*的計數器，請輸入：

```
logman create counter perf_log -c \Processor(_Total)\% Processor time
```

若要使用 [處理器（_Total）] 計數器類別的 [% Processor time] 計數器建立名為*perf_log*的計數器，建立大小上限為 10 MB 的記錄檔，並收集1分鐘和0秒的資料，請輸入：

```
logman create counter perf_log -c \Processor(_Total)\% Processor time -max 10 -rf 01:00
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman](logman.md)
