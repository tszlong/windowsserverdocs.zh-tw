---
title: 子命令開始-TransportServer
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1bdf80aa9c255e12e1e4821467d556eb67f8691
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370729"
---
# <a name="subcommand-start-transportserver"></a>子命令： start-TransportServer

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動傳輸伺服器的所有服務。
## <a name="syntax"></a>語法
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>典型
若要啟動伺服器，請輸入下列其中一項：
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 TransportServer 命令](using-the-disable-transportserver-command.md)@no__t[-3 使用](using-the-enable-transportserver-command.md)[TransportServer 命令](using-the-get-transportserver-command.md)
 子命令，
[。TransportServer](subcommand-set-transportserver.md)
[子命令： Stop-TransportServer](subcommand-stop-transportserver.md)
