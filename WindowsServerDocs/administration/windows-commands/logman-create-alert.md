---
title: logman create alert
description: Logman create alert 命令的參考文章，此命令會建立警示資料收集器。
ms.topic: reference
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9b21a9e9a633518a1d7b7a51915ee0a8af8f47c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025312"
---
# <a name="logman-create-alert"></a>logman create alert

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立警示資料收集器。

## <a name="syntax"></a>語法

```
logman create alert <[-n] <name>> [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| -s `<computer name>` | 在指定的遠端電腦上執行命令。 |
| -config `<value>` | 指定包含命令選項的設定檔。 |
| [-n] `<name>` | 目標物件的名稱。 |
| -[-] u `<user [password]>` | 指定要執行的使用者。 輸入 `*` 密碼會產生密碼提示。 當您在密碼提示字元中輸入密碼時，不會顯示該密碼。 |
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
| -cf `<filename>` | 指定要收集的效能計數器清單。 檔案應包含每行一個效能計數器名稱。 |
| -[-] el | 啟用或停用事件日誌報告。 |
| -th `<threshold [threshold [...]]>` | 指定計數器及其警示的臨界值。 |
| -[-]rdcs `<name>` | 指定當警示引發時，要啟動的資料收集器集合。 |
| -[-] tn `<task>` | 指定當警示引發時要執行的工作。 |
| -[-]targ `<argument>` | 指定要搭配使用-tn 指定之工作使用的工作引數。 |
| /? | 顯示即時線上說明。 |

#### <a name="remarks"></a>備註

- 列出 [-] 時，新增額外的連字號 (-) 會否定選項。

### <a name="examples"></a>範例

若要建立稱為的新警示 *new_alert*，當處理器 (_Total) 計數器群組中的效能計數器% processor time 超過計數器值50時，就會引發此警示，請輸入：

```
logman create alert new_alert -th \Processor(_Total)\% Processor time>50
```

> [!NOTE]
> 定義的臨界值是根據計數器所收集的值，因此在此範例中，50的值等於50% 的處理器時間。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [logman 更新警示命令](logman-update-alert.md)

- [logman 命令](logman.md)
