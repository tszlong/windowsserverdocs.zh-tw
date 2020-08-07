---
title: logman update alert
description: Logman 更新警示命令的參考文章，可更新現有警示資料收集器的屬性。
ms.topic: article
ms.assetid: ede94a76-931c-40ed-9fda-6766bed8ff72
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1139b3d5f8e49c70f00b5bd5780a96f30bafa41e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887258"
---
# <a name="logman-update-alert"></a>logman update alert

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

更新現有警示資料收集器的屬性。

## <a name="syntax"></a>語法

```
logman update alert <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s`<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config`<value>` | 指定包含命令選項的設定檔案。 |
| [-n]`<name>` | 目標物件的名稱。 |
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
| -[-] my.cnf`<[[hh:]mm:]ss>` | 當指定時間時，會在指定的時間已過時建立新的檔案。 未指定時間時，會在超過最大大小時建立新的檔案。 |
| -y | 對所有問題回答 [是] 而不提示。 |
| -cf`<filename>` | 指定要收集的效能計數器清單。 檔案每行應該包含一個效能計數器名稱。 |
| -[-] el | 啟用或停用事件記錄檔報告。 |
| -th`<threshold [threshold [...]]>` | 指定警示的計數器及其臨界值。 |
| -[-]rdcs`<name>` | 指定在引發警示時要啟動的資料收集器集合。 |
| -[-] tn`<task>` | 指定警示引發時要執行的工作。 |
| -[-]targ`<argument>` | 指定要與使用-tn 指定的工作搭配使用的工作引數。 |
| /? | 顯示即時線上說明。 |

#### <a name="remarks"></a>備註

- 其中列出 [-]，加入額外的連字號 (-) 會將選項否定。

### <a name="examples"></a>範例

若要更新名為*new_alert*的現有警示，請在 [處理器 (_Total) ] 計數器群組中，將 [計數器% Processor time] 的臨界值設定為40%，輸入：

```
logman update alert new_alert -th \Processor(_Total)\% Processor time>40
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 建立警示命令](logman-create-alert.md)

- [logman 命令](logman.md)
