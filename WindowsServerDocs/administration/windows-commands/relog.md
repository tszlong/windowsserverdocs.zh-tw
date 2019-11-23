---
title: relog
description: 瞭解如何從效能 coutner 記錄檔中解壓縮效能計數器資訊。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: daedd85f1557c191a690e7eb750559cfd268d3a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371620"
---
# <a name="relog"></a>relog

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從效能計數器記錄檔中將效能計數器解壓縮成其他格式，例如文本-TSV （適用于 tab 鍵分隔的文字）、text-CSV （以逗號分隔的文字）、binary BIN 或 SQL。   

## <a name="syntax"></a>語法  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Parameters  

|                                         參數                                          |                                                                                                                                                                  描述                                                                                                                                                                   |
|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                *檔案名*[*filename ...* ]                                 |                                                                                                                      指定現有效能計數器記錄檔的路徑名稱。 您可以指定多個輸入檔案。                                                                                                                      |
|                                             -a                                             |                                                                                                          附加輸出檔，而不是覆寫。 此選項不適用於預設一律附加的 SQL 格式。                                                                                                           |
|                                   -c*路徑*[*path ...* ]                                   |                                                       指定要記錄的效能計數器路徑。 若要指定多個計數器路徑，請以空格分隔，並以引號括住計數器路徑（例如， **"** <em>Counterpath1</em> <em>Counterpath2</em> **"** ）                                                       |
|                                       -cf*檔案名*                                       |                                            指定文字檔的路徑名稱，列出要包含在重新登錄檔案中的效能計數器。 使用此選項可列出輸入檔中的計數器路徑，每行一個。 預設設定是原始記錄檔中的所有計數器都是 relogged。                                            |
|                                  -f {bin\| csv\|tsv\|SQL}                                  |                                       指定輸出檔案格式的路徑名稱。 預設格式為**bin**。 若是 SQL 資料庫，輸出檔案會指定*DSN！CounterLog*。 您可以使用 ODBC 管理員來設定 DSN （資料庫系統名稱），以指定資料庫位置。                                        |
|                                         -t*值*                                         |                                                                                                           指定 "*N*" 記錄中的取樣間隔。 包含重新登錄檔案中的每 n 個資料點。 預設值為每個資料點。                                                                                                           |
| -o {*OutputFile* \| *"SQL： DSN！Counter_Log*}，其中 DSN 是在系統上定義的 ODMC dsn。 |                                                   指定要寫入計數器的輸出檔或 SQL 資料庫的路徑名稱。 <br>注意：如果是64位和32位版本的重新登錄，您必須在 ODBC 資料來源中定義 DSN （分別為64位和32位）                                                   |
|                          -b \<*M*/*D*/*YYYY*> [[*HH*：]*MM*：]*SS*                           |                                                                          指定從輸入檔複製第一筆記錄的開始時間。 日期和時間必須是此精確格式<em>M</em> **/** <em>D</em> **/** <em>YYYYHH</em> **：** <em>MM</em> **：** <em>SS</em>。                                                                          |
|                          -e \<*M*/*D*/*YYYY*> [[*HH*：]*MM*：]*SS*                           |                                                                           指定從輸入檔複製最後一筆記錄的結束時間。 日期和時間必須是此精確格式<em>M</em> **/** <em>D</em> **/** <em>YYYYHH</em> **：** <em>MM</em> **：** <em>SS</em>。                                                                            |
|                                -config {*FileName* \| *i*}                                 | 指定包含命令列參數之設定檔案的路徑名稱。 在設定檔中使用 *-i*作為可放在命令列上的輸入檔清單的預留位置。 不過，在命令列上，您不需要使用*i*。 您也可以使用萬用字元（例如 \*.blg）來指定許多輸入檔名稱。 |
|                                             -q                                             |                                                                                                                          顯示輸入檔中指定之記錄檔的效能計數器和時間範圍。                                                                                                                           |
|                                             -y                                             |                                                                                                                                            針對所有問題回答「是」，略過提示。                                                                                                                                             |
|                                             /?                                             |                                                                                                                                                      在命令提示字元顯示說明。                                                                                                                                                      |

## <a name="remarks"></a>備註  
計數器路徑格式：  
- 計數器路徑的一般格式如下： [\\\<computer >] \\\<物件 > [\<父系 >\\< 實例 # Index >] \\\<Counter >]，其中格式的父系、實例、索引和計數器元件可能包含有效的名稱或萬用字元。 所有計數器都不需要電腦、父系、實例和索引元件。  
- 您可以根據計數器本身來決定要使用的計數器路徑。 例如，LogicalDisk 物件具有 <Index>的實例，因此您必須提供 < #index > 或萬用字元。 因此，您可以使用下列格式： **\LogicalDisk （\*/\*#\*）\\\\** *  
- 相較之下，Process 物件不需要 \<索引 > 實例。 因此，您可以使用下列格式： **\Process （\*） \ID Process**  
- 如果在父名稱中指定萬用字元，則會傳回符合指定之實例和計數器欄位的指定物件的所有實例。  
- 如果在實例名稱中指定萬用字元，則當對應至指定索引的所有實例名稱符合萬用字元時，就會傳回指定之物件和父物件的所有實例。  
- 如果在計數器名稱中指定萬用字元，則會傳回指定之物件的所有計數器。  
- 不支援部分計數器路徑字串相符專案（例如，pro *）。  

計數器檔案：  
-   計數器檔案是文字檔，會列出現有記錄檔中的一或多個效能計數器。 從記錄檔或 \<電腦中的 **/q**輸出複製完整的計數器名稱 >\\\<物件 >\\\<實例 >\\\< 計數器 > 格式。 列出每一行上的一個計數器路徑。  

複製計數器：  
-   執行時，重新記錄會從輸入檔中的每一筆**記錄複製指定**的計數器，並在必要時轉換格式。 計數器檔案中允許使用萬用字元路徑。  
儲存輸入檔子集：  
-   使用 **/t**參數來指定輸入檔會依照每個 <n>筆記錄的間隔插入輸出檔案中。 根據預設，資料會從每一筆記錄中 relogged。  
搭配使用 **/b**和 **/e**參數與記錄檔  
-   您可以指定您的輸出記錄包含早于開始時間（也就是 **/b**）的記錄，以提供需要格式化值之計算值的計數器資料。 輸出檔案會有輸入檔中的最後一筆記錄，其時間戳記小於 **/e** （也就是結束時間）參數。  
使用 **/config**選項：  
-   搭配 **/config**選項使用的設定檔案內容應具有下列格式：  
    -   \<CommandOption >\\\<值 >，其中 \<CommandOption > 是命令列選項，\<值 > 指定其值。

如需將**重新執行納入**WINDOWS MANAGEMENT INSTRUMENTATION （WMI）腳本的詳細資訊，請參閱[Microsoft Windows 資源套件網站](https://go.microsoft.com/fwlink/?LinkId=4665)上的「編寫 WMI 腳本」。  

## <a name="BKMK_Examples"></a>典型  
若要對現有的追蹤記錄進行重新取樣，請將固定間隔設為30，列出計數器路徑、輸出檔案和格式：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
若要對現有的追蹤記錄進行重新取樣，請將固定間隔設為30，列出計數器路徑和輸出檔案：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
若要將現有的追蹤記錄重新取樣到資料庫，請使用：
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
