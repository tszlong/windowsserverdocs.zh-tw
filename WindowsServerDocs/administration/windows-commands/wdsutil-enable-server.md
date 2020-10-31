---
title: wdsutil 啟用-伺服器
description: Wdsutil enable-server 命令的參考文章，可啟用 Windows 部署服務的所有服務。
ms.topic: reference
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e2f62198ce4c012245174bc00e536e15ecd1c27
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070320"
---
# <a name="wdsutil-enable-server"></a>wdsutil 啟用-伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用 Windows 部署服務的所有服務。

## <a name="syntax"></a>語法

```
wdsutil [options] /Enable-Server [/Server:<Servername>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。 |

## <a name="examples"></a>範例

若要在伺服器上啟用服務，請輸入下列其中一項：

```
wdsutil /Enable-Server
```

```
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil disable-server 命令](wdsutil-disable-server.md)

- [wdsutil get-Server 命令](wdsutil-get-server.md)

- [wdsutil initialize-server 命令](wdsutil-initialize-server.md)

- [wdsutil 設定-server 命令](wdsutil-set-server.md)

- [wdsutil 啟動-伺服器命令](wdsutil-start-server.md)

- [wdsutil stop-server 命令](wdsutil-stop-server.md)

- [wdsutil 解除初始化-server 命令](wdsutil-uninitialize-server.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
