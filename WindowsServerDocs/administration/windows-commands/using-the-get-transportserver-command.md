---
title: TransportServer
description: TransportServer 的參考文章，它會顯示指定之傳輸伺服器的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115942290679decd8b8c660e4113576efb30123d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932169"
---
# <a name="get-transportserver"></a>TransportServer

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定之傳輸伺服器的相關資訊。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Config}|傳回指定之傳輸伺服器的相關設定資訊。|
## <a name="examples"></a>範例
若要查看伺服器的相關資訊，請輸入：
```
wdsutil /Get-TransportServer /Show:Config
```
若要查看設定資訊，請輸入：
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 TransportServer 命令](using-the-disable-transportserver-command.md) 
[使用 TransportServer 命令](using-the-enable-transportserver-command.md) 
[子命令： set-TransportServer](subcommand-set-transportserver.md) 
[子命令： start-TransportServer](subcommand-start-transportserver.md) 
[子命令： stop-TransportServer](subcommand-stop-transportserver.md)
