---
title: 啟用-伺服器
description: 啟用-伺服器的參考主題，這會啟用所有服務以進行 Windows 部署服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf7bf57c0784fa16719b9f77da50212bca0ef850
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720927"
---
# <a name="enable-server"></a>啟用-伺服器

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用 Windows 部署服務的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要啟用伺服器上的服務，請執行下列其中一項：
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [Command-Line Syntax Key](command-line-syntax-key.md) 
使用[停](using-the-disable-server-command.md)
 
 
 
 [ ](using-the-get-server-command.md) [ ](using-the-initialize-server-command.md) [ ](subcommand-start-server.md) [ ](the-uninitialize-server-option.md) [ ](subcommand-set-server.md) [Subcommand: stop-Server](subcommand-stop-server.md)用伺服器命令的命令列語法索引鍵使用 Initialize-server 命令子命令： set-server 子命令： start-server 子命令： stop-server 解除初始化-伺服器選項
 

