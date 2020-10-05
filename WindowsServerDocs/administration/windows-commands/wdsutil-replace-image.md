---
title: wdsutil 取代-影像
description: Wdsutil 取代映射的參考文章，會以該映射的新版本取代現有的映射。
ms.topic: reference
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b03bf133fa84f1a37578889ef8f3b3fefd5a8d16
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729855"
---
# <a name="wdsutil-replace-image"></a>wdsutil 取代-影像

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以該映射的新版本取代現有的映射。
## <a name="syntax"></a>Syntax
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
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-image 命令](wdsutil-add-image.md)
- [wdsutil 複製映射命令](wdsutil-copy-image.md)
- [wdsutil 匯出-image 命令](wdsutil-export-image.md)
- [wdsutil 取得映射命令](wdsutil-get-image.md)
- [wdsutil 取代-image 命令](wdsutil-replace-image.md)
- [wdsutil 設定-image 命令](wdsutil-set-image.md)
