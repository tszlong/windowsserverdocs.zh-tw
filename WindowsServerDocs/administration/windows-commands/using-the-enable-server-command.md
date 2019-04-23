---
title: 使用命令啟用伺服器
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdf03a778a6c646aa79c2f844212b1728c5c73eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852719"
---
# <a name="using-the-enable-server-command"></a>使用命令啟用伺服器

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

可讓 Windows 部署服務的所有服務。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>範例
若要啟用在伺服器上的服務，執行下列其中一項：
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用停用伺服器命令](using-the-disable-server-command.md)
[使用 get Server 命令](using-the-get-server-command.md)
[使用初始化伺服器命令](using-the-initialize-server-command.md)
[子命令： 設定伺服器](subcommand-set-server.md)
[子命令： 啟動 Server](subcommand-start-server.md) 
 [子命令： 停止伺服器](subcommand-stop-server.md)
[取消初始化伺服器選項](the-uninitialize-server-option.md)
