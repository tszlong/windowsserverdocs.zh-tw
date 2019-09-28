---
title: logman query
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6acf6cf5240dd59357f4c788577190699a354744
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374425"
---
# <a name="logman-query"></a>logman query

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

查詢資料收集器或資料收集器集合屬性。  

## <a name="syntax"></a>語法  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>參數  

|     參數      |                                 描述                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       顯示即時線上說明。                       |
| -s <computer name> |            在指定的遠端電腦上執行命令。             |
|  -config <value>   |           指定包含命令選項的設定檔案。            |
|    [-n] <name>     |                          目標物件的名稱。                          |
|        -ets        | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |

## <a name="BKMK_examples"></a>典型  
下列命令會列出目標系統上設定的所有資料收集器集合。  
```  
logman query  
```  
下列命令會列出包含在資料收集器集合中名為 perf_log 的資料收集器。  
```  
logman query "perf_log"  
```  
下列命令會列出目標系統上所有可用的資料收集器提供者。  
```  
logman query providers  
```  
#### <a name="additional-references"></a>其他參考  
[logman](logman.md)  
