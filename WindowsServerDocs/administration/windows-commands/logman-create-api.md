---
title: logman 建立 api
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ecc0a75-2613-464a-8616-c5dc404bb736
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 512602213fcfd95770af0e27b721a589ed489771
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374605"
---
# <a name="logman-create-api"></a>logman 建立 api

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立 API 追蹤資料收集器。  

## <a name="syntax"></a>語法  
```  
logman create api <[-n] <name>> [options]  
```  
## <a name="parameters"></a>參數  

|                    參數                     |                                                                               描述                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    顯示即時線上說明。                                                                     |
|                -s <computer name>                |                                                          在指定的遠端電腦上執行命令。                                                          |
|                 -config <value>                  |                                                         指定包含命令選項的設定檔案。                                                         |
|                   [-n] <name>                    |                                                                       目標物件的名稱。                                                                        |
| -f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql > |                                                            指定資料收集器的記錄格式。                                                             |
|             -[-] u < 使用者 [password] >              | 指定要當做執行身分的使用者。 輸入密碼的 \* 會產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示它。 |
|    -m < [start] [stop] [[start] [stop] [...]]>    |                                                變更為 [手動啟動] 或 [停止]，而不是排程的開始或結束時間。                                                 |
|                -rf < [[hh：] mm：] ss >                |                                                        在指定的時間內執行資料收集器。                                                         |
|        -b < M/d/yyyy h:mm： ss [AM&#124;PM] >         |                                                              在指定的時間開始收集資料。                                                               |
|        -e < M/d/yyyy h:mm： ss [AM&#124;PM] >         |                                                               在指定的時間結束資料收集。                                                                |
|                -si < [[hh：] mm：] ss >                |                                                 指定效能計數器資料收集器的取樣間隔。                                                  |
|              -o < 路徑&#124;dsn！ log >              |                                              指定 SQL 資料庫中的輸出記錄檔或 DSN 和記錄集名稱。                                               |
|                      -[-] r                       |                                                  在指定的開始和結束時間，每日重複資料收集器。                                                  |
|                      -[-] a                       |                                                                     附加至現有的記錄檔。                                                                     |
|                      -[-] 允許                      |                                                                     覆寫現有的記錄檔。                                                                     |
|           -[-] v < nnnnnn&#124;mmddhhmm >           |                                                   將檔案版本設定資訊附加至記錄檔名稱的結尾。                                                   |
|                  -[-] rc <task>                   |                                                         每次關閉記錄檔時，執行指定的命令。                                                          |
|                 -[-] max <value>                  |                                                 最大記錄檔大小（MB）或 SQL 記錄檔的最大記錄數目。                                                  |
|              -[-] my.cnf < [[hh：] mm：] ss >              |     當指定時間時，請在指定的時間已過時建立新的檔案。 未指定時間時，請在超過最大大小時建立新的檔案。     |
|                        -y                        |                                                             對所有問題回答 [是] 而不提示。                                                              |
|            -mods < 路徑 [path [...]]>             |                                                          指定要從中記錄 API 呼叫的模組清單。                                                           |
|     -inapis < module！ api [module！ api [...]]>      |                                                         指定要包含在記錄中的 API 呼叫清單。                                                          |
|     -exapis < module！ api [module！ api [...]]>      |                                                        指定要排除在記錄之外的 API 呼叫清單。                                                         |
|                     -[-] ano                      |                                                     僅記錄（-ano） API 名稱，或不記錄（-ano） API 名稱。                                                     |
|                  -[-] 遞迴                   |                                          記錄（-遞迴）或不要以遞迴方式記錄（-遞迴） Api 超過第一層。                                           |
|                   -exe <value>                   |                                                        為 API 追蹤指定可執行檔的完整路徑。                                                        |

## <a name="remarks"></a>備註  
其中列出 [-]，此選項會有額外的否定。  
## <a name="BKMK_examples"></a>典型  
下列命令會針對可執行檔 c:\windows\notepad.exe 建立名為 trace_notepad 的 API 追蹤計數器，並將結果輸出到 file c:\notepad.etl。  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -o c:\notepad.etl  
```  
下列命令會建立名為 trace_notepad 的 API 追蹤計數器，以供可執行檔 c:\windows\notepad.exe 收集模組 c:\windows\system32\advapi32.dll. 所產生的值  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -mods c:\windows\system32\advapi32.dll  
```  
下列命令會建立名為 trace_notepad 的 API 追蹤計數器，以作為 c:\windows\notepad.exe 模組 kernel32.dll 所產生之 API 呼叫 TlsGetValue 的可執行檔。  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
#### <a name="additional-references"></a>其他參考  
[logman](logman.md)  
