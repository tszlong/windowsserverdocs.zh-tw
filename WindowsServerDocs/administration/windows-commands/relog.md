---
title: relog
description: 重新記錄命令的參考文章，此命令會從效能計數器記錄檔中解壓縮效能計數器資訊。
ms.topic: reference
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 563bd7a460ee8809ca4020f9a83f28df435127b8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027419"
---
# <a name="relog"></a>relog

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將效能計數器記錄檔中的效能計數器解壓縮成其他格式，例如文字 TSV 的 (，用於以定位字元分隔的文字) 、以逗號分隔的文字) 、二進位 BIN 或 SQL 的文字 CSV (。

>[!NOTE]
>如需將 **重新執行併入** WINDOWS MANAGEMENT INSTRUMENTATION (WMI) 腳本的詳細資訊，請參閱腳本撰寫 [blog](https://devblogs.microsoft.com/scripting/)。

## <a name="syntax"></a>語法

```
relog [<filename> [<filename> ...]] [/a] [/c <path> [<path> ...]] [/cf <filename>] [/f  {bin|csv|tsv|SQL}] [/t <value>] [/o {outputfile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<filename>|i}] [/q]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `filename [filename ...]` | 指定現有效能計數器記錄檔的路徑名稱。 您可以指定多個輸入檔。 |
| -a | 附加輸出檔，而不是覆寫。 此選項不適用於 SQL 格式，預設值為 [一律附加]。 |
| -c `path [path ...]` | 指定要記錄的效能計數器路徑。 若要指定多個計數器路徑，請以空格分隔，然後以引號括住計數器路徑 (例如 `"path1 path2"`) 。 |
| -cf 檔案名 | 指定文字檔的路徑名稱，這個文字檔會列出要包含在重新登錄檔案中的效能計數器。 使用這個選項可列出輸入檔中的計數器路徑，每行一個。 預設設定為 relogged 原始記錄檔中的所有計數器。 |
| -f `{bin | csv | tsv | SQL}` | 指定輸出檔案格式的路徑名稱。 預設格式為 **bin**。 若為 SQL 資料庫，輸出檔會指定 `DSN!CounterLog` 。 您可以使用 ODBC 管理員設定 DSN (資料庫系統名稱) 來指定資料庫位置。 |
| -t 值 | 指定 *n* 筆記錄中的取樣間隔。 包含重新登錄檔案中的第 n 個資料點。 預設值為每個資料點。 |
| -o `{Outputfile | SQL:DSN!Counter_Log}` | 指定將寫入計數器的輸出檔案或 SQL 資料庫的路徑名稱。 <P>**注意：** 若為64位和32位版本的 relog.exe，您必須分別在 ODBC 資料來源中定義 (64 位和32位的 DSN) 系統上。 使用 "SQL Server" ODBC 驅動程式來定義 DSN。 |
| -b `<M/D/YYYY> [[<HH>:]<MM>:]<SS>]` | 指定從輸入檔複製第一筆記錄的開始時間。 日期和時間的格式必須正確： M/D/YYYYHH： MM： SS。 |
| -e `<M/D/YYYY> [[<HH>:]<MM>:]<SS>]` | 指定從輸入檔複製最後一筆記錄的結束時間。 日期和時間的格式必須正確： M/D/YYYYHH： MM： SS。 |
| -config `{filename | i}` | 指定包含命令列參數之設定檔的路徑名稱。 如果您使用的是設定檔，您可以使用 **-i** 作為預留位置，以取得可放置於命令列的輸入檔清單。 如果您使用的是命令列，請不要使用 **-i**。 您也可以使用萬用字元，例如 `*.blg` 一次指定多個輸入檔名稱。 |
| -Q | 顯示輸入檔中所指定記錄檔的效能計數器和時間範圍。 |
| -y | 針對所有問題回答「是」，略過提示。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 計數器路徑的一般格式如下： `[\<computer>] \<object>[<parent>\<instance#index>] \<counter>]` 其中，格式的父系、實例、索引和計數器元件可能包含有效的名稱或萬用字元。 並非所有計數器都需要電腦、父系、實例和索引元件。

- 您可以根據計數器本身來決定要使用的計數器路徑。 例如， **LogicalDisk** 物件有一個實例 `<index>` ，所以您必須提供 `<#index>` 或萬用字元。 因此，您可以使用下列格式： `\LogicalDisk(*/*#*)\\*` 。

- 相較之下， **Process** 物件不需要實例 `<index>` 。 因此，您可以使用下列格式： `\Process(*)\ID Process` 。

- 如果在 **父** 名稱中指定萬用字元，則會傳回符合指定之實例和計數器欄位之指定物件的所有實例。

- 如果在 **實例** 名稱中指定萬用字元，則如果對應到指定索引的所有實例名稱符合萬用字元，則會傳回指定之物件和父物件的所有實例。

- 如果在 **計數器** 名稱中指定萬用字元，則會傳回指定之物件的所有計數器。

- 部分計數器路徑字串符合 (例如，不支援 pro * ) 。

- 計數器檔案是一種文字檔，會列出現有記錄檔中的一或多個效能計數器。 從記錄檔或 **/q** 輸出格式複製完整的計數器名稱 `<computer>\<object>\<instance>\<counter>` 。 列出每一行的一個計數器路徑。

- 執行時，重新 **記錄命令會** 從輸入檔中的每個記錄複製指定的計數器，並在必要時轉換格式。 計數器檔案中允許使用萬用字元路徑。

- 使用 **/t** 參數指定輸入檔會依每一筆記錄的間隔插入至輸出檔案 `nth` 。 根據預設，會從每個記錄中 relogged 資料。

- 您可以指定您的輸出記錄包含開始時間之前的記錄 (也就是說， **/b**) 為需要格式化值計算值的計數器提供資料。 輸出檔案會有輸入檔中的最後一筆記錄，其時間戳記小於 **/e** (也就是結束時間) 參數。

- 與 **/config** 選項搭配使用的設定檔內容應該具有下列格式： `<commandoption>\<value>` ，其中 `<commandoption>` 是命令列選項，並 `<value>` 指定其值。

##<a name="q-examples"></a>Q # 範例

若要在固定間隔的30個間隔重新取樣現有的追蹤記錄，請列出計數器路徑、輸出檔和格式，然後輸入：

```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv
```

若要將現有的追蹤記錄重新取樣為30的固定間隔，請列出計數器路徑和輸出檔，並輸入：

```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30
```

若要將現有的追蹤記錄重新取樣至資料庫，請輸入：

```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
