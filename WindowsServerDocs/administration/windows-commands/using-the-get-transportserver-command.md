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
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 282b69162cf3550c5bcba3282b60f15072c96ed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363069"
---
# <a name="using-the-get-transportserver-command"></a>使用 TransportServer 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定之傳輸伺服器的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/Show： {Config}|傳回指定之傳輸伺服器的相關設定資訊。|
## <a name="BKMK_examples"></a>典型
若要查看伺服器的相關資訊，請輸入：
```
wdsutil /Get-TransportServer /Show:Config
```
若要查看設定資訊，請輸入：
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 TransportServer 命令](using-the-disable-transportserver-command.md)
[使用 TransportServer 命令](using-the-enable-transportserver-command.md)
[子](subcommand-set-transportserver.md)命令： TransportServer 
[子命令：TransportServer](subcommand-start-transportserver.md)
[子命令： Stop-TransportServer](subcommand-stop-transportserver.md)
