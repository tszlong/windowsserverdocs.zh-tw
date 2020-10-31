---
title: wdsutil enable-transportserver
description: Wdsutil transportserver 命令的參考文章，它會啟用傳輸伺服器的所有服務。
ms.topic: reference
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b698a9fae2d730a78497fffe52dc91a68175f0f3
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070300"
---
# <a name="wdsutil-enable-transportserver"></a>wdsutil enable-transportserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用傳輸伺服器的所有服務。

## <a name="syntax"></a>語法

```
wdsutil [options] /Enable-TransportServer [/Server:<Servername>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |

## <a name="examples"></a>範例

若要在伺服器上啟用服務，請輸入下列其中一項：

```
wdsutil /Enable-TransportServer
```

```
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil disable-transportserver 命令](wdsutil-disable-transportserver.md)

- [wdsutil get-transportserver 命令](wdsutil-get-transportserver.md)

- [wdsutil 設定-transportserver 命令](wdsutil-set-transportserver.md)

- [wdsutil 開始-transportserver 命令](wdsutil-start-transportserver.md)

- [wdsutil stop-transportserver 命令](wdsutil-stop-transportserver.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
