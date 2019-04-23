---
title: logman 匯入 |匯出
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 67621b109c379cc2758b4036460b6bb82df3e3d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848299"
---
# <a name="logman-import--export"></a>logman 匯入 |匯出

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

從 XML 檔案匯入資料收集器集合工具，或匯出至 XML 檔案的 資料收集器集合工具。  
  
## <a name="syntax"></a>語法  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|-?|顯示即時線上說明。|  
|-s <computer name>|在指定的遠端電腦上執行命令。|  
|-config <value>|指定包含命令選項的設定檔。|  
|[-n] <name>|目標物件的名稱。|  
|xml <name>|匯入或匯出 XML 檔案的名稱。|  
|-ets|請將命令傳送至事件追蹤工作階段中，直接，但不儲存，或排程。|  
|-[-]u <user [password]>|執行身分的使用者。 輸入 * 產生密碼的提示字元的密碼。 當您輸入密碼的提示字元時，不會顯示密碼。|  
|-y|所有的問題是回應，而不提示。|  
## <a name="BKMK_examples"></a>範例  
下列命令從匯入 XML 檔案 c:\windows\perf_log.xml 電腦 server_1，資料收集器集合工具稱為的 perf_log。  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>其他參考資料  
[logman](logman.md)  
