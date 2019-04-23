---
title: logman 更新追蹤
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7111f7f-4162-4d1a-8e53-d766db0ede1f britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30fe475fb6a8442558493dcd5a91ecb8fcee2de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826429"
---
# <a name="logman-update-trace"></a>logman 更新追蹤

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

更新現有的事件追蹤資料收集器的屬性。  
  
## <a name="syntax"></a>語法  
```  
logman update trace <[-n] <name>> [options]  
```  
## <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/?|顯示即時線上說明。|  
|-s <computer name>|在指定的遠端電腦上執行命令。|  
|-config <value>|指定包含命令選項的設定檔。|  
|-ets|請將命令傳送至事件追蹤工作階段中，直接，但不儲存，或排程。|  
|[-n] <name>|目標物件的名稱。|  
|-f <bin&#124;bincirc&#124;csv&#124;tsv&#124;sql>|指定資料收集器記錄檔格式。|  
|-[-]u <user [password]>|指定給執行身分的使用者。 輸入 * 產生密碼的提示字元的密碼。 當您輸入密碼的提示字元時，不會顯示密碼。|  
|-m <[start] [stop] [[start] [stop] [...]]>|將變更為手動啟動或停止而不是排程的開始或結束時間。|  
|-rf < [[hh:] mm:] ss >|執行指定的一段時間的資料收集器。|  
|-b <M/d/yyyy h:mm:ss[AM&#124;PM]>|開始收集資料，在指定的時間。|  
|-e <M/d/yyyy h:mm:ss[AM&#124;PM]>|結束於指定時間的資料收集。|  
|-o <path&#124;dsn!log>|指定的輸出記錄檔或資料來源名稱和記錄集名稱在 SQL database 中。|  
|-[-]r|重複資料收集器每日於指定的開始和結束時間。|  
|-[-]a|將附加至現有的記錄檔。|  
|-[-]ow|覆寫現有的記錄檔。|  
|-[-]v <nnnnnn&#124;mmddhhmm>|將檔案版本資訊附加至記錄檔名稱的結尾。|  
|-[-]rc <task>|執行命令，指定每個記錄檔關閉的時間。|  
|-[-]max <value>|記錄檔大小上限以 MB 或最大 SQL 記錄檔的記錄數目。|  
|-[-] cnf < [[hh:] mm:] ss >|指定時間時，請在經過指定的時間之後建立新的檔案。 未指定時間時，建立新的檔案超過大小上限時。|  
|-y|所有的問題是回應，而不提示。|  
|-ct <perf&#124;system&#124;cycle>|指定事件追蹤工作階段計時類型。|  
|-ln <logger_name>|指定事件追蹤工作階段的記錄器名稱。|  
|-ft < [[hh:] mm:] ss >|指定的事件追蹤工作階段的清除計時器。|  
|-[-]p <provider [flags [level]]>|指定單一的事件追蹤提供者，才能啟用。|  
|-pf <filename>|指定列出多個啟用的事件追蹤提供者的檔案。 檔案應該是文字檔，其中包含每行一個提供者。|  
|-[-]rt|在即時模式中執行的事件追蹤工作階段。|  
|-[-]ul|以使用者模式執行的事件追蹤工作階段。|  
|-bs <value>|指定的事件追蹤工作階段緩衝區大小，以 kb 為單位。|  
|-nb <min max>|指定事件追蹤工作階段緩衝區的數目。|  
|-mode <globalsequence&#124;localsequence&#124;pagedmemory>|指定的事件追蹤工作階段記錄器模式。<br /><br />**Globalsequence**指定，事件追蹤器會將序號新增至接收每個事件，不論追蹤工作階段已收到的事件。<br /><br />**Localsequence**指定事件追蹤器加入特定追蹤工作階段在收到事件的序號。 當**localsequence**選項時，重複的序號可以跨所有工作階段存在，但會在每個追蹤工作階段內唯一。<br /><br />**Pagedmemory**指定事件追蹤其內部緩衝區配置的分頁的記憶體，而不是預設的非分頁記憶體集區使用。|  
## <a name="remarks"></a>備註  
當列出 [-] 時，額外的-變換正負號的選項。  
## <a name="BKMK_examples"></a>範例  
下列命令會更新現有的資料收集器 perf_log，變更記錄大小上限設為 10 MB、 更新記錄檔格式為 CSV，以及附加格式 mmddhhmm 中的檔案版本控制。  
```  
logman update perf_log -max 10 -f csv -v mmddhhmm  
```  
#### <a name="additional-references"></a>其他參考資料  
[logman](logman.md)  
[logman 建立追蹤](logman-create-trace.md)  
