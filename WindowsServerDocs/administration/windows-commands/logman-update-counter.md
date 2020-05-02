---
title: logman 更新計數器
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 606dd6cb8d391f99b5050a28105e8bd32540becd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724299"
---
# <a name="logman-update-counter"></a>logman 更新計數器

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

更新現有的計數器資料收集器的屬性。  

## <a name="syntax"></a>語法  
```  
logman update counter <[-n] <name>> [options]  
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
|                  -cf<filename>                  |                       指定要收集的效能計數器清單。 檔案每行應該包含一個效能計數器名稱。                        |
|               -c <路徑 [path []] >               |                                                              指定要收集的效能計數器。                                                               |
|                   -sc <value>                    |                                      指定要使用效能計數器資料收集器收集的最大樣本數。                                      |

## <a name="remarks"></a>備註  
其中列出 [-]，此選項會有額外的否定。  
## <a name="examples"></a>範例  
下列命令會更新資料收集器 perf_log、將取樣間隔變更為10，並將記錄格式變更為 CSV，並以 mmddhhmm 格式將版本設定新增至記錄檔名稱。  
```  
logman update perf_log -si 10 -f csv -v mmddhhmm  
```  
## <a name="additional-references"></a>其他參考  
[logman](logman.md)  
[logman 建立計數器](logman-create-counter.md)  
