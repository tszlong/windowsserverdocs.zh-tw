---
title: 解除初始化-伺服器
description: 解除初始化伺服器的參考文章，這會在初始伺服器設定期間還原對伺服器所做的變更。
ms.topic: reference
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: da747dd06ca9621e4261edc436eea48fb2a3917f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640471"
---
# <a name="uninitialize-server"></a>解除初始化-伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

還原初始伺服器設定期間對伺服器所做的變更。 這包括由 **/initialize-server** 選項或 Windows 部署服務 mmc 嵌入式管理單元所做的變更。 請注意，此命令會將伺服器重設為未設定的狀態。 此命令不會修改 remoteInstall 共用資料夾的內容。 相反地，它會重設伺服器的狀態，讓您可以重新初始化伺服器。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要重新初始化伺服器，請輸入下列其中一項：
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 disable-Server 命令](using-the-disable-server-command.md) 
[使用 enable-Server 命令](using-the-enable-server-command.md) 
[使用 get-Server 命令](using-the-get-server-command.md) 
[使用 Initialize-Server 命令](using-the-initialize-server-command.md) 
[子命令：設定伺服器](subcommand-set-server.md) 
[子命令：啟動-伺服器](subcommand-start-server.md) 
[子命令： stop-Server](subcommand-stop-server.md)
