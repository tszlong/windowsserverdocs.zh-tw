---
title: 解除初始化-伺服器選項
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c63e09738871c5b74c1b564a83c35ad28f4fa80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385586"
---
# <a name="the-uninitialize-server-option"></a>解除初始化-伺服器選項

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

還原初始伺服器設定期間對伺服器所做的變更。 這包括 **/initialize-server**選項或 Windows 部署服務 mmc 嵌入式管理單元所做的變更。 請注意，此命令會將伺服器重設為未設定的狀態。 此命令不會修改 remoteInstall 共用資料夾的內容。 相反地，它會重設伺服器的狀態，讓您可以重新初始化伺服器。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>典型
若要重新初始化伺服器，請輸入下列其中一項：
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 disable-server 命令](using-the-disable-server-command.md)
[ 使用可使用伺服器命令](using-the-enable-server-command.md)
[使用取得伺服器命令](using-the-get-server-command.md)
[使用初始化伺服器命令 ](using-the-initialize-server-command.md)
[子命令：設定-伺服器](subcommand-set-server.md)
[子命令： Start-server](subcommand-start-server.md)3[子命令： stop-server](subcommand-stop-server.md)
