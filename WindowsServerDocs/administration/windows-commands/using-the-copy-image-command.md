---
title: 使用複製映射命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2f269884e9c1f96151c9e1220242fad917b76831
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363563"
---
# <a name="using-the-copy-image-command"></a>使用複製映射命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

複製位於相同映射群組內的影像。 若要複製映射群組之間的影像，請使用 [[匯出-映射命令](using-the-export-image-command.md)] 命令，然後使用 [[新增映射命令](using-the-add-image-command.md)] 命令。
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
媒體： <Image name>|指定要複製之影像的名稱。|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體：安裝|指定要複製的影像類型。 此選項必須設定為 [**安裝**]。|
|\mediaGroup： <Image group name>]|指定包含要複製之影像的映射群組。 如果未指定映射群組，而且伺服器上只存在一個群組，則預設會使用該映射群組。 如果伺服器上有一個以上的映射群組，您就必須指定映射群組。|
|[/Filename： <Filename>]|指定要複製之影像的檔案名。 如果來源映射無法以名稱唯一識別，您就必須指定檔案名。|
|/DestinationImage|指定目的地映射的設定，如下表所述。<br /><br />-/Name： <Name>-設定要複製之影像的顯示名稱。<br />-/Filename： <Filename>-設定將包含影像複製的目的地影像檔案名稱。<br />-[/Description： <Description>]-設定影像複製的描述。|
## <a name="BKMK_examples"></a>典型
若要建立指定映射的複本，並將它命名為 WindowsVista，請輸入：
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
若要建立指定映射的複本，請套用指定的設定，並將複本命名為 WindowsVista，輸入：
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
 使用[新增映射](using-the-add-image-command.md)命令 
 使用 [[匯出-](using-the-export-image-command.md)映射] 命令，@no__t[-5，](using-the-get-image-command.md)並使用 
 的 [[移除映射](using-the-remove-image-command.md)] 命令 
[取代-影像命令](using-the-replace-image-command.md)1[子命令：設定-影像](subcommand-set-image.md)
