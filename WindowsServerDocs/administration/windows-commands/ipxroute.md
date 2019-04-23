---
title: ipxroute
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d995204eea0af776a2084a82411fa95542d1d77a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889089"
---
# <a name="ipxroute"></a>ipxroute

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示及修改路由資料表使用 IPX 通訊協定的相關資訊。 不含參數， **ipxroute**會顯示傳送至未知、 廣播和多點傳送位址的封包的預設設定。   
## <a name="syntax"></a>語法  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|servers[ /type=X]|顯示指定的伺服器類型的服務存取點 (SAP) 資料表。  **X**必須是整數。 例如， **/類型 = 4**會顯示所有檔案伺服器。 如果您未指定 **/type**， **ipxroute 伺服器**會顯示所有類型的伺服器，其列出的伺服器名稱。|  
|ripout 網路|如果探索*網路*是連線到諮詢 IPX 堆疊的路由表，然後送出 rip 要求，如有必要。  *網路*是 IPX 網路區段數字。|  
|resolve{ GUID&#124; name} { GUID&#124; AdapterName}|解析 GUID 的名稱好記的名稱或其 GUID 的易記名稱。|  
|面板 = *N*|指定要查詢或設定參數的網路介面卡。|  
|def|所有路由廣播傳送封包。 如果封包傳送到唯一的媒體存取 (MAC) 位址不在來源路由表**ipxroute**廣播預設的單一路由傳送封包。|  
|gbr|所有路由廣播傳送封包。 如果封包傳送到的廣播位址 (FFFFFFFFFFFF)， **ipxroute**廣播預設的單一路由傳送封包。|  
|mbr|所有路由廣播傳送封包。 如果封包傳送到多點傳送位址 (C000xxxxxxxx)， **ipxroute**廣播預設的單一路由傳送封包。|  
|remove= *xxxxxxxxxxxx*|從來源路由表移除指定的節點位址。|  
|設定|顯示所有設定 IPX 的繫結的相關資訊。|  
|/?|在命令提示字元顯示說明。|  
## <a name="BKMK_Examples"></a>範例  
若要顯示工作站連接的網路區段，工作站節點的位址及所使用的畫面格型別，請輸入：  
```  
ipxroute config  
```  
## <a name="additional-references"></a>其他參考資料  
-   [命令列語法關鍵](command-line-syntax-key.md)  
