---
title: 子命令開始-TransportServer
description: 命令 TransportServer 的 Windows 命令主題，它會啟動傳輸伺服器的所有服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1356d415006324d75783d4e12ad6882d0fcc779
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833751"
---
# <a name="subcommand-start-transportserver"></a>子命令： start-TransportServer

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動傳輸伺服器的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要啟動伺服器，請輸入下列其中一項：
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
使用[TransportServer 命令](using-the-disable-transportserver-command.md)
使用[TransportServer 命令](using-the-enable-transportserver-command.md)
[使用 TransportServer 命令](using-the-get-transportserver-command.md)
[子命令： Set-TransportServer](subcommand-set-transportserver.md)
[子命令： stop-TransportServer](subcommand-stop-transportserver.md)
