---
title: 啟用-伺服器
description: 啟用-伺服器的 Windows 命令主題，可啟用所有服務以進行 Windows 部署服務。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b10ff920667cfdbaae5baaf096bf56e11ce880e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831541"
---
# <a name="enable-server"></a>啟用-伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟用 Windows 部署服務的所有服務。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要啟用伺服器上的服務，請執行下列其中一項：
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
[使用
使用伺服器命令](using-the-get-server-command.md)的[[停](using-the-disable-server-command.md)用伺服器] 命令
[使用 [初始化-伺服器] 命令](using-the-initialize-server-command.md)
[子命令：設定-伺服器](subcommand-set-server.md)
[子命令：啟動伺服器](subcommand-start-server.md)
[子命令： stop-server](subcommand-stop-server.md)
[[解除初始化-伺服器] 選項](the-uninitialize-server-option.md)
