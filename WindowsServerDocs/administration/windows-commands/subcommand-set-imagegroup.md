---
title: 子命令集-ImageGroup
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10023e493ae4db51783b7401c12bc1605145b86c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370794"
---
# <a name="subcommand-set-imagegroup"></a>子命令： set-ImageGroup

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更映射群組的屬性。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup： <Image group name>|指定映射群組的名稱。|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定，將會使用本機伺服器。|
|[/Name： <New image group name>]|指定映射群組的新名稱。|
|[/Security： <SDDL>]|以安全描述項定義語言（SDDL）格式指定映射群組的新安全描述項。|
## <a name="BKMK_examples"></a>典型
若要設定映射群組的名稱，請輸入：
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:"New Image Group Name"
```
若要指定映射群組的各種設定，請輸入：
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:"New Image Group Name" 
/Security:"O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)"
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)@no__t-@no__t[1 使用](using-the-add-imagegroup-command.md)[AllImageGroups 命令](using-the-get-allimagegroups-command.md)在-3 使用[ImageGroup 命令](using-the-get-imagegroup-command.md)，
，請使用[ImageGroup 命令](using-the-remove-imagegroup-command.md)

