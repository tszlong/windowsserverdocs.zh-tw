---
title: logman 更新 api
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ce75cf12d2fe51a0e81962f94b3a32b8d15961e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837069"
---
# <a name="logman-update-api"></a>logman 更新 api

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

更新現有的 API 追蹤資料收集器的屬性。  
  
## <a name="syntax"></a>語法  
```  
logman update api <[-n] <name>> [options]  
```  
## <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|/?|顯示即時線上說明。|  
|-s <computer name>|在指定的遠端電腦上執行命令。|  
|-config <value>|指定包含命令選項的設定檔。|  
|[-n] <name>|目標物件的名稱。|  
|-f <bin&#124;bincirc&#124;csv&#124;tsv&#124;sql>|指定資料收集器記錄檔格式。|  
|-[-]u <user [password]>|指定給執行身分的使用者。 輸入 * 產生密碼的提示字元的密碼。 當您輸入密碼的提示字元時，不會顯示密碼。|  
|-m <[start] [stop] [[start] [stop] [...]]>|將變更為手動啟動或停止而不是排程的開始或結束時間。|  
|-rf < [[hh:] mm:] ss >|執行指定的一段時間的資料收集器。|  
|-b <M/d/yyyy h:mm:ss[AM&#124;PM]>|開始收集資料，在指定的時間。|  
|-e <M/d/yyyy h:mm:ss[AM&#124;PM]>|結束於指定時間的資料收集。|  
|si < [[hh:] mm:] ss >|指定效能計數器資料收集器的取樣間隔時間。|  
|-o <path&#124;dsn!log>|指定的輸出記錄檔或資料來源名稱和記錄集名稱在 SQL database 中。|  
|-[-]r|重複資料收集器每日於指定的開始和結束時間。|  
|-[-]a|將附加至現有的記錄檔。|  
|-[-]ow|覆寫現有的記錄檔。|  
|-[-]v <nnnnnn&#124;mmddhhmm>|將檔案版本資訊附加至記錄檔名稱的結尾。|  
|-[-]rc <task>|執行命令，指定每個記錄檔關閉的時間。|  
|-[-]max <value>|記錄檔大小上限以 MB 或最大 SQL 記錄檔的記錄數目。|  
|-[-] cnf < [[hh:] mm:] ss >|指定時間時，請在經過指定的時間之後建立新的檔案。 未指定時間時，建立新的檔案超過大小上限時。|  
|-y|所有的問題是回應，而不提示。|  
|-遊戲外掛 < 路徑 [路徑 [...]]>|指定要記錄的 API 呼叫的模組清單。|  
|-inapis < 模組 ！ api [模組 ！ api [...]]>|指定要包含在記錄中的 API 呼叫的清單。|  
|-exapis < 模組 ！ api [模組 ！ api [...]]>|指定 API 呼叫來記錄從排除的清單。|  
|-[-]ano|記錄檔 (-ano) API 的名稱，或不只會記錄 (-ano) API 名稱。|  
|-[-]recursive|記錄檔 (-遞迴) 或不記錄 (-遞迴) Api 以遞迴方式之外的第一道防線。|  
|-exe <value>|指定 API 追蹤的可執行檔的完整路徑。|  
## <a name="remarks"></a>備註  
當列出 [-] 時，額外的-變換正負號的選項。  
## <a name="BKMK_examples"></a>範例  
下列命令更新現有的 API 追蹤的計數器會藉由排除 API 呼叫所產生的模組 kernel32.dll TlsGetValue 呼叫可執行檔 c:\windows\notepad.exe trace_notepad。  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
#### <a name="additional-references"></a>其他參考資料  
[logman](logman.md)  
[logman 建立 api](logman-create-api.md)  
