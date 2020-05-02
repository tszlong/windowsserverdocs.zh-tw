---
title: 新增-ImageGroup
description: ImageGroup 的參考主題，它會將映射群組新增至 Windows 部署服務伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b08042ac6b33c0ccfe0b66bb0fec70805d55d75f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721046"
---
# <a name="add-imagegroup"></a>新增-ImageGroup

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將映射群組新增至 Windows 部署服務伺服器。

## <a name="syntax"></a>語法
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup:<Image group name>|指定要加入之映射群組的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要新增映射群組，請輸入下列其中一項：
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md)
[AllImageGroups 命令](using-the-get-allimagegroups-command.md)
使用[ImageGroup 命令](using-the-get-imagegroup-command.md)
，並[使用 ImageGroup 命令](using-the-remove-imagegroup-command.md)
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)
