---
title: logman delete
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8360a4955a5ebed3eb25fda77acf587c56fbf5d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374433"
---
# <a name="logman-delete"></a>logman delete

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除現有的資料收集器。  

## <a name="syntax"></a>語法  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>參數  

|        參數        |                                                                               描述                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    顯示即時線上說明。                                                                     |
|   -s <computer name>    |                                                          在指定的遠端電腦上執行命令。                                                          |
|     -config <value>     |                                                         指定包含命令選項的設定檔案。                                                         |
|       [-n] <name>       |                                                                   目標資料收集器的名稱。                                                                    |
|          -ets           |                                              直接將命令傳送至事件追蹤會話，而不儲存或排程。                                               |
| -[-] u < 使用者 [password] > | 指定要當做執行身分的使用者。 輸入密碼的 \* 會產生密碼的提示。 當您在密碼提示字元中輸入密碼時，不會顯示它。 |

## <a name="BKMK_examples"></a>典型  
下列命令會刪除資料收集器 perf_log。  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>其他參考  
[logman](logman.md)  
