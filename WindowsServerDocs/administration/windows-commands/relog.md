---
title: relog
description: 了解如何從效能命名的計數器記錄檔中擷取效能計數器資訊。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6804c25af04907edc8180b6a37be7efcc470f259
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869359"
---
# <a name="relog"></a>relog

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取效能計數器記錄的效能計數器儲存成其他格式，例如文字-TSV （適用於 tab 鍵分隔的文字）、 文字-CSV （適用於以逗號分隔的文字）、 二進位分類收納或 SQL。   

## <a name="syntax"></a>語法  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>參數  

|參數|描述|
|--|--|
|*FileName* [*FileName ...*]|指定現有的效能計數器記錄檔的路徑名稱。 您可以指定多個輸入的檔案。|
|-a |將附加輸出檔，而非覆寫。 此選項不適用於 SQL 的格式，要附加一律為預設值。  |
|-c *path* [*path ...*]|指定要記錄的效能計數器路徑。 若要指定多個計數器路徑，請以空格分隔，並以引號括住的計數器路徑 (例如，**」 * * * Counterpath1* * Counterpath2 ***"**)|  
|-cf *FileName*|指定文字檔，其中列出要包含在重新登錄檔案中的效能計數器的路徑的名稱。 使用此選項可將計數器路徑清單輸入檔中的，每行一個。 預設值是在原始記錄檔中的所有計數器重新都記錄。|  
|-f {bin\| csv\|tsv\|SQL}|指定輸出檔格式的路徑名稱。 預設的格式是**bin**。 針對 SQL 資料庫中，指定輸出檔*DSN ！CounterLog*。 您可以使用 ODBC 管理員設定 DSN （資料庫系統名稱） 來指定資料庫位置。  |
|-t*值*|指定取樣間隔中的 「*N*」 記錄。 包含每個第 n 個資料點中重新登錄檔案。 預設值為每個資料點。|  
|-o {*OutputFile* \| *"SQL:DSN ！Counter_Log*} DSN 是 ODMC DSN 的系統上所定義。|指定輸出檔或 SQL 資料庫將在其中寫入計數器的路徑名稱。 <br>注意：Relog.exe 64 位元和 32 位元版本，您需要定義 ODBC 資料來源中的 DSN (64 位元和 32 位元分別)|
|-b \< *M*/*D*/*YYYY*> [[*HH*:]*MM*:]*SS*|指定開始複製輸入檔中的第一筆記錄的時間。 日期和時間必須以這個確切的格式*M***/*** D***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|-e \< *M*/*D*/*YYYY*> [[*HH*:]*MM*:]*SS* |指定複製輸入檔中的最後一筆記錄的結束的時間。 日期和時間必須以這個確切的格式*M***/*** D***/*** YYYYHH ***:*** MM ***:*** SS*.|  
|-config {*FileName* \| *i*}|指定包含命令列參數的設定檔的路徑名稱。 使用 *-i*可以放在命令列的輸入檔清單做為預留位置的組態檔中。 在命令列中，不過，您不需要使用*我*。 您也可以使用萬用字元，例如 *.blg 來指定許多輸入的檔案名稱。|  
|-q|顯示的效能計數器和時間範圍的記錄在輸入檔中指定的檔案。|  
|-y|略過提示回答 [是] 所有問題。|  
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註  
計數器路徑格式：  
-   計數器路徑的一般格式如下: [\\\<電腦 >] \\\<物件 > [\<父 >\\< 執行個體 #Index >] \\ \<計數器 >] 其中父系、 執行個體、 索引和格式的計數器元件可能會包含有效的名稱或萬用字元。 電腦、 父系、 執行個體和索引元件不需要所有的計數器。  
-   您決定要使用的計數器路徑計數器本身為基礎。 比方說，邏輯磁碟物件擁有執行個體<Index>，因此您必須提供 < #index > 或萬用字元。 因此，您可以使用下列格式： **\LogicalDisk (\*/\*#\*)\\\***  
-   相較之下，處理程序物件不需要執行個體\<索引 >。 因此，您可以使用下列格式： **\Process (\*) \ID 程序**  
-   如果父系名稱中指定萬用字元，則會傳回符合指定的執行個體和計數器欄位指定之物件的所有執行個體。  
-   如果執行個體名稱中指定萬用字元，如果指定索引相對應的所有執行個體名稱符合萬用字元，將會傳回在指定的物件的父物件的所有執行個體。  
-   如果計數器名稱中指定萬用字元，則會傳回指定之物件的所有計數器。  
-   不支援部分的計數器路徑字串相符項目 （例如，pro *）。  

計數器的檔案：  
-   計數器檔案是文字檔，列出一或多個現有的記錄檔中的效能計數器。 從記錄檔複製完整的計數器名稱或 **/q**輸出\<電腦 >\\\<物件 >\\\<執行個體 >\\ \<計數器 > 格式。 在每一行列出一個計數器路徑。  

複製的計數器：  
-   在執行時，**重新記錄**複製指定的計數器從每一筆記錄在輸入檔中，如有必要，請將轉換的格式。 在計數器檔案中允許使用萬用字元的路徑。  
正在儲存輸入的檔案子集：  
-   使用 **/t**參數來指定輸入的檔會插入到輸出檔時間間隔的每個<n>個記錄。 根據預設，資料會從每一筆記錄重新記錄。  
使用 **/b**並 **/e**與記錄檔的參數  
-   您可以指定您的輸出記錄檔包含從開始時間之前的記錄 (亦即**b**) 以提供資料給要求的格式化值的計算值的計數器。 輸出檔將會有的最後一個記錄，從輸入檔具有時間戳記小於 **/e** （也就是結束時間） 參數。  
使用 **/config**選項：  
-   搭配使用的設定檔的內容 **/config**選項應該具有下列格式：  
    -   \<CommandOption >\\\<的值 >，其中\<CommandOption > 是命令列選項和\<值 > 指定其值。

如需有關併入**重新記錄**到您的 Windows Management Instrumentation (WMI) 指令碼，請參閱 < 指令碼的 WMI > 網址[Microsoft Windows Resource Kits 網站](https://go.microsoft.com/fwlink/?LinkId=4665)。  

## <a name="BKMK_Examples"></a>範例  
若要對現有的追蹤記錄檔重新取樣在固定的間隔為 30，列出計數器路徑，輸出檔案和格式：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
若要對現有的追蹤記錄檔重新取樣在固定的間隔為 30，列出計數器路徑和輸出檔案：  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
若要對現有的追蹤記錄檔重新取樣成資料庫使用：
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
<!---
-   The following is a list of the possible formats:  
    -   \<computer>\\\<Object>(\<Parent>/\<Instance#Index>)\<Counter>  
    -   \<computer>\<Object>(<Parent>/<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance#Index>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>\\<Counter>  
    -   \\<Object>(<Parent>/<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Parent>/<Instance>)<Counter>  
    -   \\<Object>(<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Instance>)\\<Counter>  
    -   \\<Object>\\<Counter>  
--->