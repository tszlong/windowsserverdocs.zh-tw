---
title: wdsutil 停止-伺服器
description: 子命令停止-伺服器的參考文章，會停止 Windows 部署服務伺服器上的所有服務。
ms.topic: reference
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0c1288900cdc5eaa46c27b6eb05fd64b9a1b16a4
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729854"
---
# <a name="wdsutil-stop-server"></a>wdsutil 停止-伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止 Windows 部署服務伺服器上的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要停止服務，請輸入下列其中一項：
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil disable-server 命令](wdsutil-disable-server.md)
- [wdsutil enable-server 命令](wdsutil-enable-server.md)
- [wdsutil get-server 命令](wdsutil-get-server.md)
- [wdsutil initialize-server 命令](wdsutil-initialize-server.md)
- [wdsutil 設定-server 命令](wdsutil-set-server.md)
- [wdsutil 啟動-伺服器命令](wdsutil-start-server.md)
- [wdsutil 解除初始化-server 命令](wdsutil-uninitialize-server.md)

