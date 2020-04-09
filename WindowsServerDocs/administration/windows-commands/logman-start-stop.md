---
title: logman 開始 |停止
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bd81a33779aa58e7528d0173a7a4b49489de8f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840621"
---
# <a name="logman-start--stop"></a>logman 開始 |停止

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動資料收集器並將開始時間設為手動，或停止資料收集器集合，並將結束時間設為手動。  

## <a name="syntax"></a>語法  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
### <a name="parameters"></a>參數  

|     參數      |                                 描述                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       顯示即時線上說明。                       |
| -s <computer name> |            在指定的遠端電腦上執行命令。             |
|  -config <value>   |           指定包含命令選項的設定檔案。            |
|    [-n] <name>     |                          目標物件的名稱。                          |
|        -ets        | 直接將命令傳送至事件追蹤會話，而不儲存或排程。 |
|        -as         |               以非同步方式執行要求的作業。                |

## <a name="examples"></a><a name=BKMK_examples></a>典型  
下列命令會啟動遠端電腦 server_1 上的資料收集器 perf_log。  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>其他參考資料  
[logman](logman.md)  
