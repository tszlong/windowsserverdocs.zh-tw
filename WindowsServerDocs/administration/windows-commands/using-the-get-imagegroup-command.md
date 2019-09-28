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
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6531a3b69840a0a4910b2effdd3e349b76edf2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363125"
---
# <a name="using-the-get-imagegroup-command"></a>使用 ImageGroup 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取映射群組及其內影像的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup： <Image group name>|指定映射群組的名稱。|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/detailed]|傳回每個影像的影像中繼資料。 如果此參數不是使用，則預設行為是只傳回映射名稱、描述和檔案名。|
## <a name="BKMK_examples"></a>典型
若要查看映射群組的相關資訊，請輸入：
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
若要查看包含中繼資料的資訊，請輸入：
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
 使用[AllImageGroups 命令](using-the-get-allimagegroups-command.md)在-3 使用[ImageGroup @no__t 命令](using-the-add-imagegroup-command.md)，
[使用 ImageGroup 命令](using-the-remove-imagegroup-command.md)
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)
