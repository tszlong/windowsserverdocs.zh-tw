---
title: 使用 get ImageGroup 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9dcb76155dc1044730673ed46a53cad57441a246
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862599"
---
# <a name="using-the-get-imagegroup-command"></a>使用 get ImageGroup 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取映像群組和其內的映像的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaGroup:<Image group name>|指定映像群組名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|[/ 詳細]|傳回每個映像的映像中繼資料。 如果未使用此參數，預設行為是傳回僅映像名稱、 描述和檔案名稱。|
## <a name="BKMK_examples"></a>範例
若要檢視映像群組的相關資訊，請輸入：
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
若要檢視包含的中繼資料的資訊，請輸入：
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增 ImageGroup 命令](using-the-add-imagegroup-command.md)
[使用 get AllImageGroups 命令](using-the-get-allimagegroups-command.md)
 [使用移除 ImageGroup 指令](using-the-remove-imagegroup-command.md)
[子命令： 集 ImageGroup](subcommand-set-imagegroup.md)
