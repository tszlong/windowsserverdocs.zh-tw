---
title: 取代-影像
description: 取代映射的參考文章，會以該映射的新版本取代現有的映射。
ms.topic: reference
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 405a8465c043250ee3177febc2ceab9b50b924b4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023062"
---
# <a name="using-the-replace-image-command"></a>使用取代映射命令

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以該映射的新版本取代現有的映射。
## <a name="syntax"></a>語法
針對開機映射：
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/Name:<Image name>]
         [/Description:<Image description>]
```
針對安裝映射：
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/SourceImage:<Source image name>]
         [/Name:<Image name>]
         [/Description:<Image description>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定要取代之映射的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體媒體： {Boot &#124; Install}|指定要取代的影像類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定要取代之映射的架構。 由於不同架構的不同開機映射可以有相同的映射名稱，因此指定架構可確保取代正確的映射。|
|[/Filename： <File name> ]|如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
|/replacementImage|指定取代映射的設定。 您可以使用下列選項來設定這些設定：<p>-mediaFile： <file path> 指定新 .wim 檔案的名稱和位置 (完整路徑) 。<br />-[/SourceImage： <image name> ]-指定 .wim 檔案包含多個映射時所要使用的映射。 此選項只適用于安裝映射。<br />-[/Name： <Image name> ] 設定影像的顯示名稱。<br />-[/Description： <Image description> ]-設定影像的描述。|
## <a name="examples"></a>範例
若要取代開機映射，請輸入下列其中一項：
```
wdsutil /replace-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:C:\MyFolder\Boot.wim
wdsutil /verbose /Progress /replace-Imagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:My WinPE Image /Description:WinPE Image with drivers
```
若要取代安裝映射，請輸入下列其中一項：
```
wdsutil /replace-Imagmedia:Windows Vista Homemediatype:Install /replacementImagmediaFile:C:\MyFolder\Install.wim
wdsutil /verbose /Progress /replace-Imagmedia:Windows Vista Pro /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:Windows Vista Ultimate /Name:Windows Vista Desktop /Description:Windows Vista Ultimate with standard business applications.
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用新增映射命令](using-the-add-image-command.md) 
[使用複製映射命令](using-the-copy-image-command.md) 
[使用 Export-Image 命令](using-the-export-image-command.md) 
[使用取得映射命令](using-the-get-image-command.md) 
[使用取代映射命令](using-the-replace-image-command.md) 
[子命令：設定映射](subcommand-set-image.md)
