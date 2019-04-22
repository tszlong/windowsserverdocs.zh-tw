---
title: 使用 get AllImageGroups 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 917f61327a3d39ee97c5fd59072884f7844c487e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822349"
---
# <a name="using-the-get-allimagegroups-command"></a>使用 get AllImageGroups 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取伺服器和這些映像群組中的所有映像上所有的映像群組的相關資訊。
## <a name="syntax"></a>語法
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|[/ 詳細]|傳回從每個映像的映像的中繼資料。 如果未使用此參數，預設行為是傳回僅映像名稱、 描述和每個映像的檔案名稱。|
## <a name="BKMK_examples"></a>範例
若要檢視映像群組的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增 ImageGroup 命令](using-the-add-imagegroup-command.md)
[使用 get ImageGroup 命令](using-the-get-imagegroup-command.md)
[使用移除 ImageGroup 命令](using-the-remove-imagegroup-command.md)
[子命令： 集 ImageGroup](subcommand-set-imagegroup.md)
