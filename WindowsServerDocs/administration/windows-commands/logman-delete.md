---
title: logman delete
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b30fd6eb7915d3d0296988a98968dcde58bdbc2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724378"
---
# <a name="logman-delete"></a>logman delete

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除現有的資料收集器。  

## <a name="syntax"></a>語法  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>參數  

|        參數        |                                                                               描述                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    顯示即時線上說明。                                                                     |
|   -s<computer name>    |                                                          在指定的遠端電腦上執行命令。                                                          |
|     -config <value>     |                                                         指定包含命令選項的設定檔案。                                                         |
|       [-n]<name>       |                                                                   目標資料收集器的名稱。                                                                    |
|          -ets           |                                              直接將命令傳送至事件追蹤會話，而不儲存或排程。                                               |
| -[-] u <使用者 [password] > | 指定要當做執行身分的使用者。 \*輸入密碼時，會產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示它。 |

## <a name="examples"></a>範例  
下列命令會刪除資料收集器 perf_log。  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>其他參考  
[logman](logman.md)  
