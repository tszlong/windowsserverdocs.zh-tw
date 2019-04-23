---
title: ntfrsutl
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2dd245b7d85d6d5668262d3800ab72e35b852246
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836619"
---
# <a name="ntfrsutl"></a>ntfrsutl

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

內部資料表、 執行緒和 NT 檔案複寫服務的記憶體資訊傾印\(NTFRS\)。 它會針對本機和遠端伺服器執行。 NTFRS 服務控制管理員中的復原設定\(SCM\)甚至有重大影響，以找出並保留重要的記錄檔事件的電腦上。 這項工具提供方便的方法，檢閱這些設定。   
  
## <a name="syntax"></a>語法  
  
```  
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]  
ntfrsutl[memory|threads|stage][<computer>]  
ntfrsutl ds[<computer>]  
ntfrsutl [sets][<computer>]  
ntfrsutl [version][<computer>]  
ntfrsutl poll[/quickly[=[<N>]]][/slowly[=[<N>]]][/now][<computer>]  
```  
  
### <a name="parameters"></a>參數  
  
|參數|描述|  
|-------|--------|  
|idtable|資料表識別碼|  
|configtable|FRS 組態資料表|  
|inlog|輸入記錄檔|  
|outlog|輸出記錄檔|  
|<computer>|指定的電腦。|  
|memory|記憶體使用狀況|  
|執行緒||  
|階段||  
|ds|列出的 DS NTFRS 服務的檢視。|  
|設定|指定作用中複本集|  
|version|指定的 API 和 NTFRS 服務版本。|  
|輪詢|指定目前的輪詢間隔。<br /><br />參數：<br /><br /><ul><li>**\/快速**\[ **\=** \[ <N> \] \]\(快速輪詢  \)<br /><br /><ul><li>**快速**\-快速輪詢，直到穩定的設定是 rectrieved</li><li>**快速\=** \-快速每分鐘會輪詢預設值。</li><li>**快速\=** <N> \-快速輪詢每個*N*分鐘</li></ul></li><li>**\/緩時變**\[ **\=** \[ <N> \] \] \(緩時變輪詢\)<br /><br /><ul><li>**緩時變**\-輪詢緩時變，直到擷取穩定的設定</li><li>**緩時變\=** \-緩時變每分鐘會輪詢預設</li><li>**緩時變\=** <N> \-快速輪詢每個*N*分鐘</li></ul></li><li>**\/現在**\(現在輪詢\)</li></ul>|  
|\/？|在命令提示字元顯示說明。|  
  
## <a name="BKMK_Examples"></a>範例  
若要判斷檔案複寫的輪詢間隔：  
  
```  
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1  
```  
  
若要判斷目前的 NTFRS 應用程式開發介面\(API\)版本：  
  
```  
C:\Program Files\SupportTools>ntfrsutl version  
```  
  
## <a name="additional-references"></a>其他參考資料  
  
-   [命令列語法關鍵](command-line-syntax-key.md)  
  
  
  

