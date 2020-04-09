---
title: 子命令停止-伺服器
description: 命令停止伺服器的 Windows 命令主題，這會停止 Windows 部署服務伺服器上的所有服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2671e7a2c2e5bb542cecd9374ad6364d68ff5bd4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833711"
---
# <a name="subcommand-stop-server"></a>子命令： stop-Server

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止 Windows 部署服務伺服器上的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要停止服務，請輸入下列其中一項：
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
使用
[使用 啟用-](using-the-enable-server-command.md)伺服器][命令，
](using-the-disable-server-command.md) [使用伺服器命令
使用](using-the-get-server-command.md)[
子[命令：](using-the-initialize-server-command.md) [設定-伺服器](subcommand-set-server.md)
[子命令：啟動伺服器](subcommand-start-server.md)
[[解除初始化-伺服器] 選項](the-uninitialize-server-option.md)
