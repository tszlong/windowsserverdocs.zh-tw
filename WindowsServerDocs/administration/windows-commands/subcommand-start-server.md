---
title: 子命令啟動-伺服器
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78407f3474c48b928535abb652d2c023dd1c1c14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383824"
---
# <a name="subcommand-start-server"></a>子命令： start-Server

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動 Windows 部署服務伺服器的所有服務。
## <a name="syntax"></a>語法
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定要啟動的伺服器名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>典型
若要啟動伺服器，請輸入下列其中一項：
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 disable-server 命令](using-the-disable-server-command.md)
[使用可用伺服器命令](using-the-enable-server-command.md)
[使用取得伺服器命令](using-the-get-server-command.md)
[使用未初始化伺服器命令](using-the-initialize-server-command.md)
[子命令：設定-伺服器](subcommand-set-server.md)
[子命令： Stop-server](subcommand-stop-server.md)
[[解除初始化-伺服器] 選項](the-uninitialize-server-option.md)
