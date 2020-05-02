---
title: logman 建立警示
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c56b8ee9ca9aa96cbff9af6f3726c62a21d7a17
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724437"
---
# <a name="logman-create-alert"></a>logman 建立警示

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

建立警示資料收集器。  

## <a name="syntax"></a>語法  
```  
logman create alert <[-n] <name>> [options]  
```  
### <a name="parameters"></a>參數  

|                 參數                  |                                                                               描述                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    顯示即時線上說明。                                                                     |
|             -s<computer name>             |                                                          在指定的遠端電腦上執行命令。                                                          |
|              -config <value>               |                                                         指定包含命令選項的設定檔案。                                                         |
|                [-n]<name>                 |                                                                       目標物件的名稱。                                                                        |
|          -[-] u <使用者 [password] >           | 指定要當做執行身分的使用者。 \*輸入密碼時，會產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示它。 |
| -m < [start] [stop] [[start] [stop] [...]]> |                                                變更為 [手動啟動] 或 [停止]，而不是排程的開始或結束時間。                                                 |
|             -rf < [[hh：] mm：] ss>             |                                                        在指定的時間內執行資料收集器。                                                         |
|     -b <M/d/yyyy h:mm： ss [AM&#124;PM] >      |                                                              在指定的時間開始收集資料。                                                               |
|     -e <M/d/yyyy h:mm： ss [AM&#124;PM] >      |                                                               在指定的時間結束資料收集。                                                                |
|             -si < [[hh：] mm：] ss>             |                                                 指定效能計數器資料收集器的取樣間隔。                                                  |
|           -o <路徑&#124;dsn！ log>           |                                              指定 SQL 資料庫中的輸出記錄檔或 DSN 和記錄集名稱。                                               |
|                   -[-] r                    |                                                  在指定的開始和結束時間，每日重複資料收集器。                                                  |
|                   -[-] a                    |                                                                     附加至現有的記錄檔。                                                                     |
|                   -[-] 允許                   |                                                                     覆寫現有的記錄檔。                                                                     |
|        -[-] v <nnnnnn&#124;mmddhhmm>        |                                                   將檔案版本設定資訊附加至記錄檔名稱的結尾。                                                   |
|               -[-] rc<task>                |                                                         每次關閉記錄檔時，執行指定的命令。                                                          |
|              -[-] max <value>               |                                                 最大記錄檔大小（MB）或 SQL 記錄檔的最大記錄數目。                                                  |
|           -[-] my.cnf < [[hh：] mm：] ss>           |     當指定時間時，請在指定的時間已過時建立新的檔案。 未指定時間時，請在超過最大大小時建立新的檔案。     |
|                     -y                     |                                                             對所有問題回答 [是] 而不提示。                                                              |
|               -cf<filename>               |                       指定要收集的效能計數器清單。 檔案每行應該包含一個效能計數器名稱。                        |
|                   -[-] el                   |                                                                啟用或停用事件記錄檔報告。                                                                 |
|     -第 <閾值 [閾值 [...]]>      |                                                        指定警示的計數器及其臨界值。                                                        |
|              -[-]rdcs<name>               |                                                     指定在引發警示時要啟動的資料收集器集合。                                                      |
|               -[-] tn<task>                |                                                             指定警示引發時要執行的工作。                                                              |
|            -[-]targ<argument>             |                                               指定要與使用-tn 指定的工作搭配使用的工作引數。                                                |

## <a name="remarks"></a>備註  
其中列出 [-]，此選項會有額外的否定。  
## <a name="examples"></a>範例  
下列命令會建立稱為 new_alert 的警示，而當處理器（_Total）計數器群組中的效能計數器時間超過計數器值50時，就會引發此警示。  
```  
logman create alert new_alert -th \Processor(_Total)\% Processor time>50  
```  
> [!NOTE]
> 定義的臨界值是以計數器所收集的值為基礎，因此在此範例中，50的值等於50% 的處理器時間。  
> ## <a name="additional-references"></a>其他參考  
> [logman](logman.md)  
