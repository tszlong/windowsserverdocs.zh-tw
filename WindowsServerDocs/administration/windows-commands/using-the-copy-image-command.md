---
title: 使用 [複製影像] 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52c9c0bb45e60e76077bf90534e93f2c6fc1df18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884039"
---
# <a name="using-the-copy-image-command"></a>使用 [複製影像] 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

相同的映像群組內的複本映像。 若要複製映像群組之間的映像，請使用[使用匯出映像命令](using-the-export-image-command.md)命令，然後[使用新增映像命令](using-the-add-image-command.md)命令。
如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
## <a name="syntax"></a>語法
```
wdsutil [Options] /copy-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定要複製之影像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
mediatype:Install|指定要複製的影像類型。 此選項必須設定為**安裝**。|
|\mediaGroup:<Image group name>]|指定包含要複製映像的映像群組。 如果未指定任何映像群組，而且只有一個群組存在於伺服器上，預設將會使用該映像群組。 如果伺服器上有一個以上的映像群組，您必須指定映像群組。|
|[/ Filename:<Filename>]|指定要複製的映像的檔案名稱。 如果名稱不能唯一識別來源映像，您必須指定檔案名稱。|
|/DestinationImage|下表中所述，請指定目的地映像的設定。<br /><br />-/Name:<Name> -設定要複製的映像的顯示名稱。<br />-/Filename:<Filename> -設定將會包含映像複本的目的地映像檔案的名稱。<br />-[/ 描述： <Description>]-設定映像複本的描述。|
## <a name="BKMK_examples"></a>範例
若要建立一份指定的映像並將它命名為 WindowsVista.wim，輸入：
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
若要建立一份指定的映像，將套用指定的設定，並將命名複製 WindowsVista.wim，型別：
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增映像命令](using-the-add-image-command.md)
[使用 匯出映像命令](using-the-export-image-command.md)
[使用取得映像的命令](using-the-get-image-command.md)
[移除映像命令](using-the-remove-image-command.md)
[使用 [取代影像] 命令](using-the-replace-image-command.md)
 [子命令： 設定影像](subcommand-set-image.md)
