---
title: 使用 get TransportServer 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 08aa1273d09ba92de15e13f7bfcc8283ac2fedb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817419"
---
# <a name="using-the-get-transportserver-command"></a>使用 get TransportServer 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示指定的傳輸伺服器的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/ 顯示: {Config}|傳回指定的傳輸伺服器的組態資訊。|
## <a name="BKMK_examples"></a>範例
若要檢視伺服器的相關資訊，請輸入：
```
wdsutil /Get-TransportServer /Show:Config
```
若要檢視設定資訊，請輸入：
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用停用 TransportServer 命令](using-the-disable-transportserver-command.md)
[使用啟用 TransportServer 命令](using-the-enable-transportserver-command.md)
 [子命令： Set-transportserver](subcommand-set-transportserver.md)
[子命令： 開始 TransportServer](subcommand-start-transportserver.md)
[子命令： 停止 TransportServer](subcommand-stop-transportserver.md)
