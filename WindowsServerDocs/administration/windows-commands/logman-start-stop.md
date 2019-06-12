---
title: logman start |停止
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c6027c4c9a99e45bb1c2e95cdfd4a7687a5c43b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437686"
---
# <a name="logman-start--stop"></a>logman start |停止

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

啟動資料收集器和開始時間設為手動，或停止資料收集器設定，並將結束時間設定為手動。  

## <a name="syntax"></a>語法  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>參數  

|     參數      |                                 描述                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       顯示即時線上說明。                       |
| -s <computer name> |            在指定的遠端電腦上執行命令。             |
|  -config <value>   |           指定包含命令選項的設定檔。            |
|    [-n] <name>     |                          目標物件的名稱。                          |
|        -ets        | 請將命令傳送至事件追蹤工作階段中，直接，但不儲存，或排程。 |
|        -as         |               以非同步方式執行要求的作業。                |

## <a name="BKMK_examples"></a>範例  
下列命令會在遠端電腦 server_1 上啟動資料收集器 perf_log。  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>其他參考資料  
[logman](logman.md)  
