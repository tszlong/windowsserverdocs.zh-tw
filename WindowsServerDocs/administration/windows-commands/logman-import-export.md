---
title: logman 匯入 |進出口
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 309274b5288bd1c17259e01cf563ae8685a2094e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374457"
---
# <a name="logman-import--export"></a>logman 匯入 |進出口

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從 XML 檔案匯入資料收集器集合檔，或將資料收集器集合匯出至 XML 檔案。  

## <a name="syntax"></a>語法  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>參數  

|        參數        |                                                                        描述                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             顯示即時線上說明。                                                              |
|   -s <computer name>    |                                                   在指定的遠端電腦上執行命令。                                                   |
|     -config <value>     |                                                  指定包含命令選項的設定檔案。                                                  |
|       [-n] <name>       |                                                                目標物件的名稱。                                                                 |
|       -xml <name>       |                                                         要匯入或匯出的 XML 檔案名。                                                         |
|          -ets           |                                       直接將命令傳送至事件追蹤會話，而不儲存或排程。                                        |
| -[-] u < 使用者 [password] > | 執行身分的使用者。 輸入密碼的 \* 會產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示它。 |
|           -y            |                                                      對所有問題回答 [是] 而不提示。                                                       |

## <a name="BKMK_examples"></a>典型  
下列命令會將 c:\windows\perf_log.xml 自電腦 server_1 的 XML 檔案匯入為名為 perf_log 的資料收集器集合。  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>其他參考  
[logman](logman.md)  
