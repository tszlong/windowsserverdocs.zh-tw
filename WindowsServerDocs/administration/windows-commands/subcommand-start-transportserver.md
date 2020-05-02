---
title: 子命令開始-TransportServer
description: 子命令 TransportServer 的參考主題，它會啟動傳輸伺服器的所有服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92bd68421883c49ec29dfb78f06121bff880b01e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721640"
---
# <a name="subcommand-start-transportserver"></a>子命令： start-TransportServer

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動傳輸伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要啟動伺服器，請輸入下列其中一項：
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [Command-Line Syntax Key](command-line-syntax-key.md)使用
[TransportServer 命令](using-the-disable-transportserver-command.md)
 [ ](using-the-enable-transportserver-command.md) 
 
 [ ](using-the-get-transportserver-command.md) [ ](subcommand-stop-transportserver.md) [Subcommand: set-TransportServer](subcommand-set-transportserver.md)的命令列語法金鑰使用 TransportServer 命令子命令： set-TransportServer 子命令： stop-TransportServer

