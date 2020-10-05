---
title: wdsutil 啟動-伺服器
description: 子命令啟動-伺服器的參考文章，會啟動 Windows 部署服務伺服器的所有服務。
ms.topic: reference
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 03791bd3a313f1ab2a5a2e076036d0aaddb18a8e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730590"
---
# <a name="wdsutil-start-server"></a>wdsutil 啟動-伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動 Windows 部署服務伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定要啟動之伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要啟動伺服器，請輸入下列其中一項：
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil disable-server 命令](wdsutil-disable-server.md)
- [wdsutil enable-server 命令](wdsutil-enable-server.md)
- [wdsutil get-server 命令](wdsutil-get-server.md)
- [wdsutil initialize-server 命令](wdsutil-initialize-server.md)
- [wdsutil 設定-server 命令](wdsutil-set-server.md)
- [wdsutil stop-server 命令](wdsutil-stop-server.md)
- [wdsutil 啟動-伺服器命令](wdsutil-start-server.md)
- [wdsutil 解除初始化-server 命令](wdsutil-uninitialize-server.md)
