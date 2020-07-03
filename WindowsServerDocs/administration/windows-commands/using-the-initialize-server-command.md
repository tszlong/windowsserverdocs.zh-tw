---
title: 初始化-伺服器
description: 初始化伺服器的參考文章，其會設定 Windows 部署服務伺服器，以在安裝伺服器角色之後進行初始使用。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22c93a07e4c2785e8cda497e9698b2031c764a42
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932165"
---
# <a name="initialize-server"></a>初始化-伺服器

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定伺服器角色安裝後，用於初始使用的 Windows 部署服務伺服器。 執行此命令之後，您應該使用，並使用 [[新增映射命令](using-the-add-image-command.md)] 命令將影像新增至伺服器。
## <a name="syntax"></a>語法
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|remInst<Full path>|指定 remoteInstall 資料夾的完整路徑和名稱。 如果指定的資料夾不存在，此選項將會在命令執行時建立。 您應該一律輸入本機路徑，即使是在遠端電腦上也一樣。 例如： **D:\remoteInstall**。|
|/Authorize|在動態主機控制通訊協定（DHCP）中授權伺服器。 只有在啟用 DHCP rogue 偵測時，才需要此選項，這表示在用戶端電腦可以服務之前，必須先在 DHCP 中授權 Windows 部署服務 PXE 伺服器。 請注意，預設會停用 DHCP rogue 偵測。|
## <a name="examples"></a>範例
若要初始化伺服器並將 remoteInstall 共用資料夾設定為 F：磁片磁碟機，請輸入。
```
wdsutil /Initialize-Server /remInst:F:\remoteInstall
```
若要初始化伺服器並將 remoteInstall 共用資料夾設為 C：磁片磁碟機，請輸入。
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:C:\remoteInstall
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 disable-Server 命令](using-the-disable-server-command.md) 
[使用 enable-Server 命令](using-the-enable-server-command.md) 
[使用 get-Server 命令](using-the-get-server-command.md) 
[子命令： set-Server](subcommand-set-server.md) 
[子命令： start-Server](subcommand-start-server.md) 
[子命令： stop-Server](subcommand-stop-server.md) 
解除[初始化-伺服器選項](the-uninitialize-server-option.md)
