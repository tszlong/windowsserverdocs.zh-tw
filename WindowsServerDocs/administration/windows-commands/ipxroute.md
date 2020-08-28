---
title: ipxroute
description: Ipxroute 命令的參考文章，此命令會顯示和修改有關 IPX 通訊協定所使用之路由表的資訊。
ms.topic: reference
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f8e9e8c376ad2b7997160b6add2b84e655a84d9
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037966"
---
# <a name="ipxroute"></a>ipxroute

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示和修改 IPX 通訊協定所使用之路由表的相關資訊。 在不使用參數的情況下使用時， **ipxroute** 會顯示傳送至未知、廣播及多播位址的封包的預設設定。

## <a name="syntax"></a>語法

```
ipxroute servers [/type=x]
ipxroute ripout <network>
ipxroute resolve {guid | name} {GUID | <adaptername>}
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]
ipxroute config
```

### <a name="parameters"></a>參數
| 參數 | 描述 |
| ------- | -------- |
| 伺服器`[/type=x]` | 針對指定的伺服器類型，顯示 (SAP) 資料表的服務存取點。 **x** 必須是整數。 例如，會 `/type=4` 顯示所有檔案伺服器。 如果您未指定 **/type**，會 `ipxroute servers` 顯示所有類型的伺服器，並依伺服器名稱列出它們。 |
| 解決 `{GUID | name}``{GUID | adaptername}` | 將 GUID 的名稱解析為其易記名稱，或將易記名稱解析成 GUID。 |
| 面板 = *n* | 指定要查詢或設定參數的網路介面卡。 |
| def | 將封包傳送到所有路由廣播。 如果封包傳送到唯一的媒體存取卡 (MAC) 位址不在來源路由表中， **ipxroute** 預設會將封包傳送至單一路由廣播。 |
| gbr | 將封包傳送到所有路由廣播。 如果封包 (FFFFFFFFFFFF) 傳送到廣播位址， **ipxroute** 預設會將封包傳送至單一路由廣播。 |
| Mbr | 將封包傳送到所有路由廣播。 如果封包傳送至多播位址 (C000xxxxxxxx) ， **ipxroute** 預設會將封包傳送至單一路由廣播。 |
| remove =* * | 從來源路由表中移除指定的節點位址。 |
| config | 顯示已設定 IPX 之所有系結的相關資訊。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要顯示工作站所連接的網路區段、工作站節點位址和使用的框架類型，請輸入：

```
ipxroute config
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
