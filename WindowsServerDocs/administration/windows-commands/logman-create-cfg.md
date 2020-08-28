---
title: logman create cfg
description: Logman create cfg 命令的參考文章，此命令會建立設定資料收集器。
ms.topic: reference
ms.assetid: bfc87093-3ff5-4e19-aa93-d185fb8e2239
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fcdd724896b48c40d88c79b5f2081cadf05acf80
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034986"
---
# <a name="logman-create-cfg"></a>logman create cfg

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立設定資料收集器。

## <a name="syntax"></a>語法

```
logman create cfg <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s `<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config `<value>` | 指定包含命令選項的設定檔。 |
| [-n] `<name>` | 目標物件的名稱。 |
| -[-] u `<user [password]>` | 指定要執行的使用者。 輸入 \* 密碼會產生密碼提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
| -m `<[start] [stop] [[start] [stop] [...]]>` | 手動啟動或停止的變更，而不是排程的開始或結束時間。 |
| -rf `<[[hh:]mm:]ss>` | 在指定的一段時間內執行資料收集器。 |
| -b `<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間開始收集資料。 |
| -e `<M/d/yyyy h:mm:ss[AM|PM]>` | 在指定的時間結束資料收集。 |
| -si `<[[hh:]mm:]ss>` | 指定效能計數器資料收集器的取樣間隔。 |
| -o `<path|dsn!log>` | 指定 SQL 資料庫中的輸出記錄檔或 DSN 和記錄集名稱。 |
| -[-] r | 每日在指定的開始和結束時間重複資料收集器。 |
| -[-] a | 附加現有的記錄檔。 |
| -[-] 允許 | 覆寫現有的記錄檔。 |
| -[-] v `<nnnnnn|mmddhhmm>` | 將檔案版本設定資訊附加至記錄檔名稱的結尾。 |
| -[-] rc `<task>` | 每次關閉記錄檔時，執行指定的命令。 |
| -[-] max `<value>` | 最大記錄檔大小（以 MB 為單位）或 SQL 記錄檔的最大記錄數目。 |
| -[-] my.cnf `<[[hh:]mm:]ss>` | 當指定時間時，會在經過指定的時間時建立新的檔案。 如果未指定時間，當超過大小上限時，就會建立新的檔案。 |
| -y | 針對所有問題回答「是」，而不提示。 |
| -[-] ni | 啟用 ( ni) 或停用 (-ni) 網路介面查詢。 |
| -reg `<path [path [...]]>` | 指定要收集的登錄值 (s) 。 |
| -進行中 `<query [query [...]]>` | 指定要使用 SQL 查詢語言收集的 WMI 物件 () 。 |
| -ftc `<path [path [...]]>` | 指定要收集之檔案的完整路徑 (s) 。 |
| /? | 顯示即時線上說明。 |

#### <a name="remarks"></a>備註

- 列出 [-] 時，新增額外的連字號 (-) 會否定選項。

### <a name="examples"></a>範例

若要建立名為 cfg_log 的設定資料收集器，請使用登錄機碼 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\` ，輸入：

```
logman create cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\\
```

若要建立稱為 cfg_log 的設定資料收集器，它會記錄 `root\wmi` 資料庫資料行中的所有 WMI 物件 `MSNdis_Vendordriverversion` ，請輸入：

```
logman create cfg cfg_log -mgt root\wmi:select * FROM MSNdis_Vendordriverversion
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman update cfg 命令](logman-update-cfg.md)

- [logman 命令](logman.md)
