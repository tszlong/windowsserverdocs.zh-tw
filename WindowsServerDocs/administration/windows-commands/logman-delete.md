---
title: logman delete
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e396d79dc936f56a69fac9469c020348640f1094
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437786"
---
# <a name="logman-delete"></a>logman delete

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

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
|     -config <value>     |                                                         指定包含命令選項的設定檔。                                                         |
|       [-n] <name>       |                                                                   目標資料收集器的名稱。                                                                    |
|          -ets           |                                              請將命令傳送至事件追蹤工作階段中，直接，但不儲存，或排程。                                               |
| -[-]u <user [password]> | 指定給執行身分的使用者。 輸入\*產生密碼的提示字元的密碼。 當您輸入密碼的提示字元時，不會顯示密碼。 |

## <a name="BKMK_examples"></a>範例  
下列命令會刪除資料收集器 perf_log。  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>其他參考資料  
[logman](logman.md)  
