---
title: 使用 TransportServer 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7531f8a638ac8fabdad08cc0134dbc63873505de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363465"
---
# <a name="using-the-disable-transportserver-command"></a>使用 TransportServer 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停用傳輸伺服器的所有服務。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定要停用之傳輸伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定傳輸伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>典型
若要停用伺服器，請輸入：
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
 使用[TransportServer 命令](using-the-get-transportserver-command.md)[的 TransportServer 命令](using-the-enable-transportserver-command.md)
，
 子命令：[Set-TransportServer](subcommand-set-transportserver.md)
[子命令：TransportServer](subcommand-start-transportserver.md)
[子命令： Stop-TransportServer](subcommand-stop-transportserver.md)
