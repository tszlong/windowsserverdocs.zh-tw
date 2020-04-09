---
title: 子命令集-ImageGroup
description: 適用于子命令集的 Windows 命令主題 ImageGroup，這會變更映射群組的屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 966e3c10d9bcc13568f5fec4e1efd1ad46a121b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833911"
---
# <a name="subcommand-set-imagegroup"></a>子命令： set-ImageGroup

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更映射群組的屬性。

## <a name="syntax"></a>語法
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup：<Image group name>|指定映射群組的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果沒有指定，則使用本機伺服器。|
|[/Name：<New image group name>]|指定映射群組的新名稱。|
|[/Security：<SDDL>]|以安全描述項定義語言（SDDL）格式指定映射群組的新安全描述項。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要設定映射群組的名稱，請輸入：
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:New Image Group Name
```
若要指定映射群組的各種設定，請輸入：
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name 
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
使用[AllImageGroups 命令](using-the-get-allimagegroups-command.md)，
使用[ImageGroup 命令](using-the-get-imagegroup-command.md)，
[使用 ImageGroup 命令](using-the-remove-imagegroup-command.md)，讓您使用[ImageGroup 命令](using-the-add-imagegroup-command.md)

