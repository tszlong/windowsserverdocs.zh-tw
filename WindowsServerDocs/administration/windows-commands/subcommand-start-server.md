---
title: 子命令啟動-伺服器
description: 子命令啟動伺服器的參考主題，它會啟動 Windows 部署服務伺服器的所有服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2a6de007e62bf3be5544f97b53a4fcc13118985
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721647"
---
# <a name="subcommand-start-server"></a>子命令： start-Server

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動 Windows 部署服務伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定要啟動的伺服器名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要啟動伺服器，請輸入下列其中一項：
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [Command-Line Syntax Key](command-line-syntax-key.md)
[disable](using-the-disable-server-command.md) 
 [ ](subcommand-stop-server.md) 
 
 [ ](using-the-get-server-command.md) 
 [ ](using-the-initialize-server-command.md) 
 [ ](subcommand-set-server.md) [ ](using-the-enable-server-command.md) [ ](the-uninitialize-server-option.md) -server 命令的命令列語法索引鍵使用 [啟用-伺服器] 命令使用 [初始化-伺服器命令] 子命令： [設定-伺服器] 子命令： [停止初始化-伺服器] 選項

