---
title: 移除-ImageGroup
description: ImageGroup 的 Windows 命令主題，它會從伺服器移除映射群組。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8406959037a958ea6d61b8e8145317635955e6d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830351"
---
# <a name="using-the-remove-imagegroup-command"></a>使用 ImageGroup 命令

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器移除映射群組。

## <a name="syntax"></a>語法
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup：<Image group name>|指定要移除之映射群組的名稱|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要移除映射群組，請輸入下列其中一項：
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer 
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)  
[使用 ImageGroup 命令](using-the-add-imagegroup-command.md)  
[使用 AllImageGroups 命令](using-the-get-allimagegroups-command.md)  
[使用 ImageGroup 命令](using-the-get-imagegroup-command.md)  
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)  
