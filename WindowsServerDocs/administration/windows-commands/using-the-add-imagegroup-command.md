---
title: 使用新增 ImageGroup 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 71050bfecdac4933bfe36f40ce09dae626735664
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829899"
---
# <a name="using-the-add-imagegroup-command"></a>使用新增 ImageGroup 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將映像群組加入至 Windows 部署服務伺服器。
## <a name="syntax"></a>語法
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup:<Image group name>|指定要加入的映像群組名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未指定伺服器名稱，則會使用本機伺服器。|
## <a name="BKMK_examples"></a>範例
若要新增映像群組，請輸入下列其中一項：
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllImageGroups 命令](using-the-get-allimagegroups-command.md)
[使用 get ImageGroup 命令](using-the-get-imagegroup-command.md)
 [使用移除 ImageGroup 指令](using-the-remove-imagegroup-command.md)
[子命令： 集 ImageGroup](subcommand-set-imagegroup.md)
