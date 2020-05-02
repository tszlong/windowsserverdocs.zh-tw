---
title: logman 建立 cfg
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bfc87093-3ff5-4e19-aa93-d185fb8e2239
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eabbbca0e13501e08efa80f19303362bc5ed687b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724409"
---
# <a name="logman-create-cfg"></a>logman 建立 cfg

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立設定資料收集器。  

## <a name="syntax"></a>語法  
```  
logman create cfg <[-n] <name>> [options]  
```  
### <a name="parameters"></a>參數  

|                    參數                     |                                                                               描述                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    顯示即時線上說明。                                                                     |
|                -s<computer name>                |                                                          在指定的遠端電腦上執行命令。                                                          |
|                 -config <value>                  |                                                         指定包含命令選項的設定檔案。                                                         |
|                   [-n]<name>                    |                                                                       目標物件的名稱。                                                                        |
| -f <bin&#124;bincirc&#124;csv&#124;tsv&#124;sql> |                                                            指定資料收集器的記錄格式。                                                             |
|             -[-] u <使用者 [password] >              | 指定要當做執行身分的使用者。 \*輸入密碼時，會產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示它。 |
|    -m < [start] [stop] [[start] [stop] [...]]>    |                                                變更為 [手動啟動] 或 [停止]，而不是排程的開始或結束時間。                                                 |
|                -rf < [[hh：] mm：] ss>                |                                                        在指定的時間內執行資料收集器。                                                         |
|        -b <M/d/yyyy h:mm： ss [AM&#124;PM] >         |                                                              在指定的時間開始收集資料。                                                               |
|        -e <M/d/yyyy h:mm： ss [AM&#124;PM] >         |                                                               在指定的時間結束資料收集。                                                                |
|                -si < [[hh：] mm：] ss>                |                                                 指定效能計數器資料收集器的取樣間隔。                                                  |
|              -o <路徑&#124;dsn！ log>              |                                              指定 SQL 資料庫中的輸出記錄檔或 DSN 和記錄集名稱。                                               |
|                      -[-] r                       |                                                  在指定的開始和結束時間，每日重複資料收集器。                                                  |
|                      -[-] a                       |                                                                     附加至現有的記錄檔。                                                                     |
|                      -[-] 允許                      |                                                                     覆寫現有的記錄檔。                                                                     |
|           -[-] v <nnnnnn&#124;mmddhhmm>           |                                                   將檔案版本設定資訊附加至記錄檔名稱的結尾。                                                   |
|                  -[-] rc<task>                   |                                                         每次關閉記錄檔時，執行指定的命令。                                                          |
|                 -[-] max <value>                  |                                                 最大記錄檔大小（MB）或 SQL 記錄檔的最大記錄數目。                                                  |
|              -[-] my.cnf < [[hh：] mm：] ss>              |     當指定時間時，請在指定的時間已過時建立新的檔案。 未指定時間時，請在超過最大大小時建立新的檔案。     |
|                        -y                        |                                                             對所有問題回答 [是] 而不提示。                                                              |
|                      -[-] ni                      |                                                         啟用（-ni）或停用（-ni）網路介面查詢。                                                          |
|             -reg <路徑 [path [...]]>             |                                                                 指定要收集的登錄值。                                                                 |
|            -<查詢 [query [...]]>            |                                                      指定要使用 SQL 查詢語言收集的 WMI 物件。                                                       |
|             -ftc <路徑 [path [...]]>             |                                                           指定要收集之檔案的完整路徑。                                                            |

## <a name="remarks"></a>備註  
其中列出 [-]，此選項會有額外的否定。  
## <a name="examples"></a>範例  
下列命令會使用登錄機碼 HKEY_LOCAL_MACHINE \Software\microsoft\windows server\ NT\Currentverion\\，建立稱為 cfg_log 的設定資料收集器。  
```  
logman create cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\\  
```  
下列命令會建立名為 cfg_log 的設定資料收集器，以記錄資料庫資料行 MSNdis_Vendordriverversion 中 root\wmi 的所有 WMI 物件。  
```  
logman create cfg cfg_log -mgt root\wmi:select * FROM MSNdis_Vendordriverversion  
```  
## <a name="additional-references"></a>其他參考  
[logman](logman.md)  
