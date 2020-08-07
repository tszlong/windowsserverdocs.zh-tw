---
title: 啟用-伺服器
description: 啟用-伺服器的參考文章，這會啟用所有服務以進行 Windows 部署服務。
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04884c4f4648db4ff78446048f34ea1ec609d154
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896544"
---
# <a name="enable-server"></a>啟用-伺服器

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用 Windows 部署服務的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 (FQDN) 的完整功能變數名稱。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要啟用伺服器上的服務，請執行下列其中一項：
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 disable-Server 命令](using-the-disable-server-command.md) 
[使用 get-Server 命令](using-the-get-server-command.md) 
[使用 Initialize-Server 命令](using-the-initialize-server-command.md) 
[子命令： set-Server](subcommand-set-server.md) 
[子命令： start-Server](subcommand-start-server.md) 
[子命令： stop-Server](subcommand-stop-server.md) 
解除[初始化-伺服器選項](the-uninitialize-server-option.md)
