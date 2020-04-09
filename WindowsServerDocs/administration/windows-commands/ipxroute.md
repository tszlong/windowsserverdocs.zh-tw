---
title: ipxroute
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1e011835dbdbcf7be1daca2cdfbd47c39f9355c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842061"
---
# <a name="ipxroute"></a>ipxroute

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示和修改 IPX 通訊協定所使用之路由表的相關資訊。 使用時不含參數， **ipxroute**會顯示傳送至未知、廣播和多播位址的封包的預設設定。   
## <a name="syntax"></a>語法  
```  
ipxroute servers [/type=X]  
ipxroute ripout <Network>  
ipxroute resolve {guid | name} {GUID | <AdapterName>}  
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]  
ipxroute config  
```  
#### <a name="parameters"></a>參數  
|參數|描述|  
|-------|--------|  
|伺服器 [/type = X]|顯示指定之伺服器類型的服務存取點（SAP）資料表。  **X**必須是整數。 例如， **/type = 4**會顯示所有檔案伺服器。 如果您未指定 **/type**， **ipxroute 伺服器**會顯示所有類型的伺服器，並依伺服器名稱列出它們。|  
|ripout 網路|藉由諮詢 IPX 堆疊的路由表並在必要時送出 rip 要求，探索*網路*是否可連線。  *Network*是 IPX 網路區段號碼。|  
|解析 {GUID&#124; name} {GUID&#124; AdapterName}|將 GUID 的名稱解析為易記名稱，或將易記名稱解析為其 GUID。|  
|面板 = *N*|指定要查詢或設定參數的網路介面卡。|  
|.def|將封包傳送至所有路由廣播。 如果封包傳輸至不在來源路由表中的唯一媒體存取卡（MAC）位址， **ipxroute**預設會將封包傳送到單一路由廣播。|  
|gbr|將封包傳送至所有路由廣播。 如果封包傳輸到廣播位址（FFFFFFFFFFFF）， **ipxroute**預設會將封包傳送到單一路由廣播。|  
|mbr|將封包傳送至所有路由廣播。 如果封包傳輸到多播位址（C000xxxxxxxx）， **ipxroute**預設會將封包傳送到單一路由廣播。|  
|remove = *？*|從來源路由表中移除指定的節點位址。|  
|config|顯示已設定 IPX 之所有系結的相關資訊。|  
|/?|在命令提示字元顯示說明。|  
## <a name="examples"></a><a name=BKMK_Examples></a>典型  
若要顯示工作站所連接的網路區段、工作站節點位址，以及所使用的框架類型，請輸入：  
```  
ipxroute config  
```  
## <a name="additional-references"></a>其他參考資料  
-   - [命令列語法關鍵](command-line-syntax-key.md)  
