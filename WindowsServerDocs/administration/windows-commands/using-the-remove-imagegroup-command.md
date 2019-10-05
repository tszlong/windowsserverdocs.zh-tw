---
title: 使用 ImageGroup 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30e4c2c7c5cf2668d62e96d8d2a54dc33e3d2a55
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960964"
---
# <a name="using-the-remove-imagegroup-command"></a>使用 ImageGroup 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器移除映射群組。
## <a name="syntax"></a>語法
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup： <Image group name>|指定要移除之映射群組的名稱|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>典型
若要移除映射群組，請輸入下列其中一項：
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer 
```
#### <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)  
[使用 ImageGroup 命令](using-the-add-imagegroup-command.md)  
[使用 AllImageGroups 命令](using-the-get-allimagegroups-command.md)  
[使用 ImageGroup 命令](using-the-get-imagegroup-command.md)  
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)  
