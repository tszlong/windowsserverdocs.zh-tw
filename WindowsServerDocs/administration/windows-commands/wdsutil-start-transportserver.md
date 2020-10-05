---
title: wdsutil 開始-transportserver
description: 子命令啟動 TransportServer 的參考文章，它會啟動傳輸伺服器的所有服務。
ms.topic: reference
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3aa948fb86b1b69448ac131c6894ff0c41f548e8
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730597"
---
# <a name="wdsutil-start-transportserver"></a>wdsutil 開始-transportserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動傳輸伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要啟動伺服器，請輸入下列其中一項：
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil disable-transportserver 命令](wdsutil-disable-transportserver.md)
- [wdsutil enable-transportserver 命令](wdsutil-enable-transportserver.md)
- [wdsutil get-transportserver 命令](wdsutil-get-transportserver.md)
- [wdsutil 設定-transportserver 命令](wdsutil-set-transportserver.md)
- [wdsutil stop-transportserver 命令](wdsutil-stop-transportserver.md)
