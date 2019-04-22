---
title: 使用 [取代影像] 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e396ee678e22885a50c02800d77ecea1cc5ef8ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819649"
---
# <a name="using-the-replace-image-command"></a>使用 [取代影像] 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

該映像的新版本取代現有的映像。
## <a name="syntax"></a>語法
針對開機映像：
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
安裝映像：
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
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定要取代之影像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
媒體類型: {開機&#124;安裝}|指定要取代的映像的類型。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定要取代映像架構。 因為它可能會有不同的開機映像的相同映像名稱不同的架構中，指定架構可確保正確的映像會被取代。|
|[/ Filename:<File name>]|如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。|
|/replacementImage|指定取代映像的設定。 您設定這些設定，使用下列選項：<br /><br />-mediaFile: <file path> -指定的名稱和新的.wim 檔案的位置 （完整路徑）。<br />-[/ SourceImage: <image name>]-指定要使用如果.wim 檔案包含多個映像的映像。 此選項僅適用於安裝映像。<br />-[/Name:<Image name>] 設定的映像的顯示名稱。<br />-[/ 描述：<Image description>]-設定映像的描述。|
## <a name="BKMK_examples"></a>範例
若要取代的開機映像，請輸入下列其中一項：
```
wdsutil /replace-Imagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:"C:\MyFolder\Boot.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:"My WinPE Image" /Description:"WinPE Image with drivers"
```
若要取代安裝映像，請輸入下列其中一項：
```
wdsutil /replace-Imagmedia:"Windows Vista Homemediatype:Install /replacementImagmediaFile:"C:\MyFolder\Install.wim"
wdsutil /verbose /Progress /replace-Imagmedia:"Windows Vista Pro" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:"Windows Vista Ultimate" /Name:"Windows Vista Desktop" /Description:"Windows Vista Ultimate with standard business applications."
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增映像命令](using-the-add-image-command.md)
[使用 [複製影像] 命令](using-the-copy-image-command.md)
[使用匯出映像命令](using-the-export-image-command.md)
[取得映像命令](using-the-get-image-command.md)
[使用 [取代影像] 命令](using-the-replace-image-command.md)
 [子命令： 設定影像](subcommand-set-image.md)
