---
title: relog
description: 重新記錄命令的參考文章，它會從效能計數器記錄檔中解壓縮效能計數器資訊。
ms.topic: article
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c3c60503cf725d05afd4b21ceef5f36c64c2b155
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883859"
---
# <a name="relog"></a>relog

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從效能計數器記錄檔中，將效能計數器解壓縮成其他格式，例如 tab 鍵分隔文字的文字 TSV () 、以逗號分隔的文字) 、二進位 BIN 或 SQL 的文字 CSV (。

>[!NOTE]
>如需有關將重新**執行納入**WINDOWS MANAGEMENT INSTRUMENTATION (WMI) 腳本的詳細資訊，請參閱[撰寫腳本的 blog](https://devblogs.microsoft.com/scripting/)。

## <a name="syntax"></a>語法

```
relog [<filename> [<filename> ...]] [/a] [/c <path> [<path> ...]] [/cf <filename>] [/f  {bin|csv|tsv|SQL}] [/t <value>] [/o {outputfile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<filename>|i}] [/q]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `filename [filename ...]` | 指定現有效能計數器記錄檔的路徑名稱。 您可以指定多個輸入檔案。 |
| -a | 附加輸出檔，而不是覆寫。 此選項不適用於預設一律附加的 SQL 格式。 |
| -c`path [path ...]` | 指定要記錄的效能計數器路徑。 若要指定多個計數器路徑，請以空格分隔，並將計數器路徑括在引號中 (例如， `"path1 path2"`) 。 |
| -cf 檔案名 | 指定文字檔的路徑名稱，列出要包含在重新登錄檔案中的效能計數器。 使用此選項可列出輸入檔中的計數器路徑，每行一個。 預設設定是原始記錄檔中的所有計數器都是 relogged。 |
| -f`{bin | csv | tsv | SQL}` | 指定輸出檔案格式的路徑名稱。 預設格式為**bin**。 針對 SQL 資料庫，輸出檔會指定 `DSN!CounterLog` 。 您可以使用 ODBC 管理員來指定資料庫位置，以設定 DSN (資料庫系統名稱) 。 |
| -t 值 | 指定*n*筆記錄中的取樣間隔。 包含重新登錄檔案中的每 n 個資料點。 預設值為每個資料點。 |
| -o`{Outputfile | SQL:DSN!Counter_Log}` | 指定要寫入計數器的輸出檔或 SQL 資料庫的路徑名稱。 <P>**注意：** 針對64位和32位版本的 relog.exe，您必須分別在 ODBC 資料來源中定義一個 DSN， (在系統上) 64 位和32位。 使用 "SQL Server" ODBC 驅動程式來定義 DSN。 |
| -b`<M/D/YYYY> [[<HH>:]<MM>:]<SS>]` | 指定從輸入檔複製第一筆記錄的開始時間。 日期和時間必須是格式正確的 M/D/YYYYHH： MM： SS。 |
| -e `<M/D/YYYY> [[<HH>:]<MM>:]<SS>]` | 指定要從輸入檔複製最後一筆記錄的結束時間。 日期和時間必須是格式正確的 M/D/YYYYHH： MM： SS。 |
| -config`{filename | i}` | 指定包含命令列參數之設定檔案的路徑名稱。 如果您使用設定檔，可以使用 **-i**做為可放在命令列上的輸入檔清單的預留位置。 如果您使用的是命令列，請不要使用 **-i**。 您也可以使用萬用字元（例如） `*.blg` 一次指定數個輸入檔案名。 |
| -Q | 顯示輸入檔中指定之記錄檔的效能計數器和時間範圍。 |
| -y | 針對所有問題回答「是」，略過提示。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 計數器路徑的一般格式如下： `[\<computer>] \<object>[<parent>\<instance#index>] \<counter>]` 其中，格式的父系、實例、索引和計數器元件可能包含有效的名稱或萬用字元。 所有計數器都不需要電腦、父系、實例和索引元件。

- 您可以根據計數器本身來決定要使用的計數器路徑。 例如， **LogicalDisk**物件具有實例 `<index>` ，因此您必須提供 `<#index>` 或萬用字元。 因此，您可以使用下列格式： `\LogicalDisk(*/*#*)\\*` 。

- 相較之下， **Process**物件不需要實例 `<index>` 。 因此，您可以使用下列格式： `\Process(*)\ID Process` 。

- 如果在**父**名稱中指定萬用字元，則會傳回符合指定之實例和計數器欄位的指定物件的所有實例。

- 如果在**實例**名稱中指定萬用字元，則當對應至指定索引的所有實例名稱符合萬用字元時，就會傳回指定之物件和父物件的所有實例。

- 如果在**計數器**名稱中指定萬用字元，則會傳回指定之物件的所有計數器。

- 部分計數器路徑字串符合 (例如，不支援 pro * ) 。

- 計數器檔案是文字檔，會列出現有記錄檔中的一或多個效能計數器。 從記錄檔或 **/q**輸出中複製完整的計數器名稱， `<computer>\<object>\<instance>\<counter>` 格式為。 列出每一行上的一個計數器路徑。

- 執行時，重新**記錄命令會**從輸入檔中的每一筆記錄複製指定的計數器，並在必要時轉換格式。 計數器檔案中允許使用萬用字元路徑。

- 使用 **/t**參數來指定將輸入檔案依每筆記錄的間隔插入輸出檔中 `nth` 。 根據預設，資料會從每一筆記錄中 relogged。

- 您可以指定輸出記錄包含開始時間之前的記錄 (也就是 **/b**) 為需要格式化值之計算值的計數器提供資料。 輸出檔案會有輸入檔中的最後一筆記錄，其時間戳記小於 **/e** (，也就是結束時間) 參數。

- 搭配 **/config**選項使用的設定檔內容應具有下列格式： `<commandoption>\<value>` ，其中 `<commandoption>` 是命令列選項，並 `<value>` 指定其值。

##<a name="q-examples"></a>問 # 範例

若要在固定間隔30的時間重新取樣現有的追蹤記錄，列出計數器路徑、輸出檔案和格式，請輸入：

```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv
```

若要在固定間隔30、列出計數器路徑和輸出檔的情況下重新取樣現有的追蹤記錄，請輸入：

```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30
```

若要將現有的追蹤記錄重新取樣到資料庫，請輸入：

```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
