---
title: 啟用-TransportServer
description: TransportServer 的參考主題，可啟用傳輸伺服器的所有服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50cc381b1c178628be7d135868027b4f37787cdd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720925"
---
# <a name="enable-transportserver"></a>啟用-TransportServer

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用傳輸伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要啟用伺服器上的服務，請執行下列其中一項：
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md)
使用[TransportServer 命令](using-the-disable-transportserver-command.md)
[使用 TransportServer 命令](using-the-get-transportserver-command.md)
[子命令： set-TransportServer](subcommand-set-transportserver.md)
[子命令： start-TransportServer](subcommand-start-transportserver.md)
[子命令： stop-TransportServer](subcommand-stop-transportserver.md)
