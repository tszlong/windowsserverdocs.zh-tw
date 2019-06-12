---
title: logman query
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e00e1ca7e6e090fd618af5b0ca2307bb573ab8c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437727"
---
# <a name="logman-query"></a>logman query

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

查詢資料收集器或資料收集器設定屬性。  

## <a name="syntax"></a>語法  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>參數  

|     參數      |                                 描述                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       顯示即時線上說明。                       |
| -s <computer name> |            在指定的遠端電腦上執行命令。             |
|  -config <value>   |           指定包含命令選項的設定檔。            |
|    [-n] <name>     |                          目標物件的名稱。                          |
|        -ets        | 請將命令傳送至事件追蹤工作階段中，直接，但不儲存，或排程。 |

## <a name="BKMK_examples"></a>範例  
下列命令會列出所有資料收集器集合工具設定目標系統上。  
```  
logman query  
```  
下列命令會列出包含在資料收集器集合工具 perf_log 資料收集器。  
```  
logman query "perf_log"  
```  
下列命令會列出所有可用的提供者的目標系統上的資料收集器。  
```  
logman query providers  
```  
#### <a name="additional-references"></a>其他參考資料  
[logman](logman.md)  
