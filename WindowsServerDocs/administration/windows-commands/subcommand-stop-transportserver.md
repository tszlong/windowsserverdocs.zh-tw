---
title: 子命令停止-TransportServer
description: 停止 TransportServer 的 Windows 命令主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44d62332307ffda4dcfa6af286c7b95cb12423dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833701"
---
# <a name="subcommand-stop-transportserver"></a>子命令： stop-TransportServer

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停止傳輸伺服器上的所有服務。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定任何傳輸伺服器，將會使用本機伺服器。|
## <a name="examples"></a><a name="BKMK_examples"></a>典型
若要停止服務，請輸入下列其中一項：
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
使用[TransportServer 命令](using-the-disable-transportserver-command.md)
使用[TransportServer 命令](using-the-enable-transportserver-command.md)
[使用 TransportServer 命令](using-the-get-transportserver-command.md)
[子命令： Set-TransportServer](subcommand-set-transportserver.md)
[子命令： start-TransportServer](subcommand-start-transportserver.md)
