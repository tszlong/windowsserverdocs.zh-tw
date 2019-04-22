---
title: 使用初始化伺服器命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a1f3a1c02eb21f630aaff7219610864cd1db2a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825189"
---
# <a name="using-the-initialize-server-command"></a>使用初始化伺服器命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在安裝伺服器角色之後，請設定初次使用的 Windows 部署服務伺服器。 執行此命令之後，您應該使用[使用新增映像命令](using-the-add-image-command.md)命令新增至伺服器的映像。
## <a name="syntax"></a>語法
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/remInst:"<Full path>"|指定完整路徑和 remoteInstall 資料夾的名稱。 如果指定的資料夾不存在，這個選項會在執行命令時建立其功能。 您一律應輸入本機路徑，即使遠端電腦。 例如: **D:\remoteInstall**。|
|[/ 授權]|授權伺服器中動態主機控制通訊協定 (DHCP)。 此選項為必要，只有當 DHCP rogue 偵測已啟用，也就是說，Windows 部署服務 PXE 伺服器必須有授權 DHCP 用戶端電腦可以接受服務之前。 請注意，預設會停用 DHCP rogue 偵測。|
## <a name="BKMK_examples"></a>範例
若要初始化伺服器，並設定 remoteInstall 共用的資料夾的 f： 磁碟機，請輸入。
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
若要初始化伺服器，並設定 remoteInstall 共用的資料夾 c： 磁碟機，請輸入。
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用停用伺服器命令](using-the-disable-server-command.md)
[使用 啟用伺服器命令](using-the-enable-server-command.md)
[使用取得伺服器的命令](using-the-get-server-command.md)
[子命令： 設定伺服器](subcommand-set-server.md)
[子命令： 啟動 Server](subcommand-start-server.md)
[子命令：停止伺服器](subcommand-stop-server.md)
[取消初始化伺服器選項](the-uninitialize-server-option.md)
