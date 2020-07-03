---
title: logman create api
description: Logman create api 命令的參考文章，它會建立 API 追蹤資料收集器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ecc0a75-2613-464a-8616-c5dc404bb736
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2294cb7ba7ab962dbba33b0e2612b8dee2d72004
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925437"
---
# <a name="logman-create-api"></a>logman create api

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立 API 追蹤資料收集器。

## <a name="syntax"></a>語法

```
logman create api <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| -s`<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config`<value>` | 指定包含命令選項的設定檔案。 |
| [-n]`<name>` | 目標物件的名稱。 |
| -f`<bin|bincirc>` | 指定資料收集器的記錄格式。 |
| -[-] u`<user [password]>` | 指定要當做執行身分的使用者。 輸入密碼時，會 `*` 產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
| -m`<[start] [stop] [[start] [stop] [...]]>` | 已變更為手動啟動或停止，而不是排程的開始或結束時間。 |
| -rf`<[[hh:]mm:]ss>` | 在指定的時間內執行資料收集器。 |
| -b`<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間開始收集資料。 |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間結束資料收集。 |
| -si`<[[hh:]mm:]ss>` | 指定效能計數器資料收集器的取樣間隔。 |
| -o`<path|dsn!log>` | 指定 SQL 資料庫中的輸出記錄檔或 DSN 和記錄集名稱。 |
| -[-] r | 在指定的開始和結束時間，每日重複資料收集器。 |
| -[-] a | 附加現有的記錄檔。 |
| -[-] 允許 | 覆寫現有的記錄檔。 |
| -[-] v`<nnnnnn|mmddhhmm>` | 將檔案版本設定資訊附加至記錄檔名稱的結尾。 |
| -[-] rc`<task>` | 每次關閉記錄檔時，執行指定的命令。 |
| -[-] max`<value>` | 最大記錄檔大小（MB）或 SQL 記錄檔的最大記錄數目。 |
| -[-] my.cnf`<[[hh:]mm:]ss>` | 當指定時間時，會在指定的時間已過時建立新的檔案。 未指定時間時，會在超過最大大小時建立新的檔案。 |
| -y | 對所有問題回答 [是] 而不提示。 |
| -mods`<path [path [...]]>` | 指定要從中記錄 API 呼叫的模組清單。 |
| -inapis` <module!api [module!api [...]]>` | 指定要包含在記錄中的 API 呼叫清單。 |
| -exapis`<module!api [module!api [...]]>` | 指定要排除在記錄之外的 API 呼叫清單。 |
| -[-] ano | 僅記錄（-ano） API 名稱，或不記錄（-ano） API 名稱。 |
| -[-] 遞迴 | 記錄（-遞迴）或不要以遞迴方式記錄（-遞迴） Api 超過第一層。 |
| -exe`<value>` | 為 API 追蹤指定可執行檔的完整路徑。 |
| /? | 顯示即時線上說明。 |

#### <a name="remarks"></a>備註

- 其中列出 [-]，加入額外的連字號（-）會否定選項。

### <a name="examples"></a>範例

若要建立名為 trace_notepad 的 API 追蹤計數器，針對可執行檔 c:\windows\notepad.exe，並將結果放在 c:\notepad.etl 檔案中，請輸入：

```
logman create api trace_notepad -exe c:\windows\notepad.exe -o c:\notepad.etl
```

若要建立名為 trace_notepad 的 API 追蹤計數器，針對可執行檔 c:\windows\notepad.exe，在 c:\windows\system32\advapi32.dll 收集模組所產生的值，請輸入：

```
logman create api trace_notepad -exe c:\windows\notepad.exe -mods c:\windows\system32\advapi32.dll
```

若要建立名為 trace_notepad 的 API 追蹤計數器，針對可執行檔 c:\windows\notepad.exe （不包括由模組 kernel32.dll 產生的 API 呼叫 TlsGetValue），請輸入：
```
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 更新 api 命令](logman-update-api.md)

- [logman 命令](logman.md)
