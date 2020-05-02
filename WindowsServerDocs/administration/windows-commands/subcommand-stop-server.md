---
title: 子命令停止-伺服器
description: 子命令停止伺服器的參考主題，這會停止 Windows 部署服務伺服器上的所有服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68cc73ac016e2ffded774567034801e1c11944d1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721627"
---
# <a name="subcommand-stop-server"></a>子命令： stop-Server

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止 Windows 部署服務伺服器上的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要停止服務，請輸入下列其中一項：
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [使用](command-line-syntax-key.md)
[disable](using-the-disable-server-command.md) 
 [ ](using-the-get-server-command.md) 
 [ ](using-the-initialize-server-command.md) 
 
 [ ](subcommand-start-server.md) 
 [ ](the-uninitialize-server-option.md) [ ](using-the-enable-server-command.md) [ ](subcommand-set-server.md)-server 命令的命令列語法索引鍵使用 [啟用-伺服器] 命令使用 [執行-server 命令] 子命令： [設定-伺服器] 子命令： [起始-伺服器] 取消初始化-伺服器選項

