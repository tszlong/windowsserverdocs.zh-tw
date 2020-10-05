---
title: wdsutil enable-transportserver
description: Wdsutil transportserver 的參考文章，可啟用傳輸伺服器的所有服務。
ms.topic: reference
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4d4a294c0bda291e8340a4d8ffe54a11d487e0dd
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729941"
---
# <a name="wdsutil-enable-transportserver"></a>wdsutil enable-transportserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用傳輸伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要在伺服器上啟用服務，請執行下列其中一項：
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil disable-transportserver 命令](wdsutil-disable-transportserver.md)
- [wdsutil get-transportserver 命令](wdsutil-get-transportserver.md)
- [wdsutil 設定-transportserver 命令](wdsutil-set-transportserver.md)
- [wdsutil 開始-transportserver 命令](wdsutil-start-transportserver.md)
- [wdsutil stop-transportserver 命令](wdsutil-stop-transportserver.md)
