---
title: 複製映射
description: 複製映射的參考文章，會複製相同映射群組內的影像。
ms.topic: reference
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4907aa76e17059b101a3bbeb793eb159dcf24b91
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038206"
---
# <a name="copy-image"></a>複製映射

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

複製相同映射群組內的影像。 若要在映射群組之間複製映射，請使用 [ [匯出-映射](using-the-export-image-command.md) ] 命令命令，然後使用 [ [新增映射命令](using-the-add-image-command.md) ] 命令。

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
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定要複製之映射的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體媒體：安裝|指定要複製的影像類型。 此選項必須設定為 [ **安裝**]。|
|\mediaGroup： <Image group name> ]|指定包含要複製之映射的映射群組。 如果未指定映射群組，且伺服器上只有一個群組，則預設會使用該映射群組。 如果伺服器上有一個以上的映射群組，您必須指定映射群組。|
|[/Filename： <Filename> ]|指定要複製之映射的檔案名。 如果來源映射無法依名稱唯一識別，您必須指定檔案名。|
|/DestinationImage|指定目的地映射的設定，如下表所述。<p>-/Name： <Name> -設定要複製之影像的顯示名稱。<br />-/Filename： <Filename> -設定將包含影像複製之目的地影像檔案的名稱。<br />-[/Description： <Description>]-設定影像複製的描述。|
## <a name="examples"></a>範例
若要建立指定映射的複本，並將它命名為 WindowsVista，請輸入：
```
wdsutil /copy-Imagmedia:Windows Vista with Officemediatype:Install /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim
```
若要建立指定映射的複本，請套用指定的設定，並將複製 WindowsVista 命名為 .wim，輸入：
```
wdsutil /verbose /Progress /copy-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /DestinationImage /Name:copy of Windows Vista with Office /Filename:WindowsVista.wim /Description:This is a copy of the original Windows image with Office installed
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用新增映射命令](using-the-add-image-command.md) 
[使用 Export-Image 命令](using-the-export-image-command.md) 
[使用取得映射命令](using-the-get-image-command.md) 
[使用移除映射命令](using-the-remove-image-command.md) 
[使用取代映射命令](using-the-replace-image-command.md) 
[子命令：設定映射](subcommand-set-image.md)
