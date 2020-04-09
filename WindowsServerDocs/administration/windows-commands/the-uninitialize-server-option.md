---
title: 解除初始化-伺服器
description: 解除初始化伺服器的 Windows 命令主題，這會在初始伺服器設定期間還原對伺服器所做的變更。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ed912af1ec3bc505e1e7465650a119af979b407
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833111"
---
# <a name="uninitialize-server"></a>解除初始化-伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

還原初始伺服器設定期間對伺服器所做的變更。 這包括 **/initialize-server**選項或 Windows 部署服務 mmc 嵌入式管理單元所做的變更。 請注意，此命令會將伺服器重設為未設定的狀態。 此命令不會修改 remoteInstall 共用資料夾的內容。 相反地，它會重設伺服器的狀態，讓您可以重新初始化伺服器。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要重新初始化伺服器，請輸入下列其中一項：
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
[使用](using-the-disable-server-command.md)
[使用 啟用-](using-the-enable-server-command.md)伺服器][命令
使用伺服器命令](using-the-get-server-command.md)
使用 [
子[命令：](using-the-initialize-server-command.md) [集合-](subcommand-set-server.md)伺服器
[子命令：啟動伺服器](subcommand-start-server.md)
[子命令：停止伺服器](subcommand-stop-server.md)
