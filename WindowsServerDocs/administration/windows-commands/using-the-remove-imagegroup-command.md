---
title: 移除-ImageGroup
description: 移除-ImageGroup 的參考文章，這會從伺服器移除映射群組。
ms.topic: reference
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 488f68173e5bed67f1bc484ef059b7a3ea849df1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636427"
---
# <a name="using-the-remove-imagegroup-command"></a>使用 ImageGroup 命令

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器移除映射群組。

## <a name="syntax"></a>語法
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup:<Image group name>|指定要移除之映射群組的名稱|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="examples"></a>範例
若要移除映射群組，請輸入下列其中一項：
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 ImageGroup 命令](using-the-add-imagegroup-command.md) 
[使用 AllImageGroups 命令](using-the-get-allimagegroups-command.md) 
[使用 ImageGroup 命令](using-the-get-imagegroup-command.md) 
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)
