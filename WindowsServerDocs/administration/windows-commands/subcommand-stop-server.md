---
title: 子命令停止-伺服器
description: 子命令停止-伺服器的參考文章，會停止 Windows 部署服務伺服器上的所有服務。
ms.topic: reference
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c84b95607b4cf0fb69765dbc941d7e984e43d7cf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626846"
---
# <a name="subcommand-stop-server"></a>子命令： stop-Server

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
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 disable-Server 命令](using-the-disable-server-command.md) 
[使用 enable-Server 命令](using-the-enable-server-command.md) 
[使用 get-Server 命令](using-the-get-server-command.md) 
[使用 Initialize-Server 命令](using-the-initialize-server-command.md) 
[子命令：設定伺服器](subcommand-set-server.md) 
[子命令：啟動-伺服器](subcommand-start-server.md) 
解除[初始化伺服器選項](the-uninitialize-server-option.md)
