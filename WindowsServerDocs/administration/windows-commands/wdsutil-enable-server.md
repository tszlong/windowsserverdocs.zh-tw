---
title: wdsutil 啟用-伺服器
description: Wdsutil enable-server 的參考文章，可啟用 Windows 部署服務的所有服務。
ms.topic: reference
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3ec32f2bd0d269795e1f88b967804c2bb82ac9f4
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729942"
---
# <a name="wdsutil-enable-server"></a>wdsutil 啟用-伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用 Windows 部署服務的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要在伺服器上啟用服務，請執行下列其中一項：
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md) 
[wdsutil disable-server 命令](wdsutil-disable-server.md) 
[wdsutil get-Server 命令](wdsutil-get-server.md) 
[wdsutil initialize-server 命令](wdsutil-initialize-server.md) 
[wdsutil 設定-server 命令](wdsutil-set-server.md) 
[wdsutil 啟動-伺服器命令](wdsutil-start-server.md) 
[wdsutil stop-server 命令](wdsutil-stop-server.md) 
[wdsutil 解除初始化-server 命令](wdsutil-uninitialize-server.md)
