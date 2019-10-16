---
title: 使用匯出-映射命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 838d84cb5f604eea710c364ed0d4a14efdc16fec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363391"
---
# <a name="using-the-export-image-command"></a>使用匯出-映射命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將映射存放區中的現有映射匯出至另一個 Windows 映像（.wim）檔案。
## <a name="syntax"></a>語法
開機映射：
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
針對安裝映射：
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體： <Image name>|指定要匯出之影像的名稱。|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Boot &#124; Install}|指定要匯出的影像類型。|
|\mediaGroup： <Image group name>]|指定包含要匯出之影像的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則預設會使用該映射群組。 如果伺服器上有一個以上的映射群組，則必須指定映射群組。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定要匯出之影像的架構。 因為不同架構中的開機映射可能會有相同的映射名稱，所以指定架構值可確保會傳回正確的映射。|
|[/Filename： <Filename>]|如果無法以名稱唯一識別映射，則必須指定檔案名。|
|/DestinationImage|指定目的地映射的設定。 您可以使用下列選項來指定這些設定：<br /><br />-/Filepath： <File path and name>-指定新映射的完整檔案路徑。<br />-[/Name： <Name>]-設定影像的顯示名稱。 如果未指定名稱，則會使用來源映射的顯示名稱。<br />-[/Description： <Description>]-設定影像的描述。|
|[/Overwrite： {Yes &#124;否&#124; append}]|判斷當/Filepath. 中已經存在具有該名稱的現有檔案時，是否會覆寫 **/DestinationImage**選項中指定的檔案。<br /><br />-   **是**會導致覆寫現有的檔案。<br />-   **否**（預設選項），如果已有相同名稱的檔案存在，就會發生錯誤。<br />-   **append**會導致產生的映射附加為現有 .wim 檔案內的新映射。|
## <a name="BKMK_examples"></a>典型
若要匯出開機映射，請輸入下列其中一項：
```
wdsutil /Export-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:"C:\temp\boot.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:"\\Server\Share\ExportImage.wim" /Name:"Exported WinPE image" /Description:"WinPE Image from WDS server" /Overwrite:Yes
```
若要匯出安裝映射，請輸入下列其中一項：
```
wdsutil /Export-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Filepath:"C:\Temp\Install.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:"Exported Windows image" /Description:"Windows Vista image from WDS server" /Overwrite:append
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用新增映射命令](using-the-add-image-command.md)
[使用複製映射命令](using-the-copy-image-command.md)
[使用取得影像命令](using-the-get-image-command.md)
[移除映射命令](using-the-remove-image-command.md)
[，使用取代影像命令](using-the-replace-image-command.md)
[子命令：設定-影像](subcommand-set-image.md)
