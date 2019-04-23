---
title: 取消初始化伺服器選項
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 73f1ff67331ae41fa0d88cb3a16df5095e0b6d66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873979"
---
# <a name="the-uninitialize-server-option"></a>取消初始化伺服器選項

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

還原初始伺服器設定期間與伺服器所做的變更。 這包括由所做的變更 **/initialize-server**選項或 Windows 部署服務 mmc 嵌入式管理單元。 請注意，此命令會重設伺服器未設定的狀態。 此命令不會修改 remoteInstall 共用資料夾的內容。 相反地，它會重設伺服器的狀態，以便您可以重新初始化伺服器。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>範例
若要重新初始化伺服器，請輸入下列其中一項：
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用停用伺服器命令](using-the-disable-server-command.md)
[使用 啟用伺服器命令](using-the-enable-server-command.md)
[使用取得伺服器的命令](using-the-get-server-command.md)
[使用初始化伺服器命令](using-the-initialize-server-command.md)
[子命令： 設定伺服器](subcommand-set-server.md)
 [子命令： 啟動伺服器](subcommand-start-server.md)
[子命令： 停止伺服器](subcommand-stop-server.md)
