---
title: wdsutil 停用-transportserver
description: Wdsutil disable transportserver 命令的參考文章，它會停用傳輸伺服器的所有服務。
ms.topic: reference
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4bbb177897e3a779de275957949b0dcf62e53478
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070350"
---
# <a name="wdsutil-disable-transportserver"></a>wdsutil 停用-transportserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停用傳輸伺服器的所有服務。

## <a name="syntax"></a>語法

```
wdsutil [Options] /Disable-TransportServer [/Server:<Servername>]
```

### <a name="parameters"></a>參數

|參數|描述|
|-------|--------|
|[/Server： `<Servername>` ]|指定要停用之傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定任何傳輸伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要停用伺服器，請輸入下列其中一項：

```
wdsutil /Disable-TransportServer
```

```
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil enable-transportserver 命令](wdsutil-enable-transportserver.md)

- [wdsutil get-transportserver 命令](wdsutil-get-transportserver.md)

- [wdsutil 設定-transportserver 命令](wdsutil-set-transportserver.md)

- [wdsutil 開始-transportserver 命令](wdsutil-start-transportserver.md)

- [wdsutil stop-transportserver 命令](wdsutil-stop-transportserver.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
