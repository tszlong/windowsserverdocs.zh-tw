---
title: 使用匯出映像的命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: efa9b2d09c37a383a91883ee02c995eedb2f235e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823139"
---
# <a name="using-the-export-image-command"></a>使用匯出映像的命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將現有的映像從映像存放區匯出到另一個 Windows 映像 (.wim) 檔案。
## <a name="syntax"></a>語法
針對開機映像：
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
安裝映像：
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
媒體：<Image name>|指定要匯出之影像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
媒體類型: {開機&#124;安裝}|指定要匯出的映像的類型。|
|\mediaGroup:<Image group name>]|指定的映像群組包含映像匯出。 如果沒有映像群組名稱指定，且只能有一個映像群組存在於伺服器上，預設將會使用該映像群組。 如果伺服器上有一個以上的映像群組，就必須指定映像群組。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定要匯出的映像的架構。 因為可能有不同的架構中的開機映像的相同映像名稱，指定架構值可確保將會傳回正確的映像。|
|[/ Filename:<Filename>]|如果映像無法依名稱來唯一識別，則必須指定檔案名稱。|
|/DestinationImage|指定目的端影像的設定。 您可以指定這些設定，使用下列選項：<br /><br />-/Filepath:<File path and name> -指定新的映像的完整檔案路徑。<br />-[/Name:<Name>]-設定的映像的顯示名稱。 如果未指定名稱，則會使用來源映像的顯示名稱。<br />-[/ 描述： <Description>]-設定映像的描述。|
|[覆寫 /: {[是]&#124;否&#124;附加}]|決定是否在指定的檔案 **/DestinationImage**如果 /Filepath 端已有同名的現有檔案，將會覆寫選項。<br /><br />-   **是**會導致系統覆寫現有的檔案。<br />-   **否**（預設選項） 會導致錯誤發生，如果已經存在具有相同名稱的檔案。<br />-   **附加**會導致產生的映像附加做為新的映像，在現有的.wim 檔案內。|
## <a name="BKMK_examples"></a>範例
若要匯出的開機映像，請輸入下列其中一項：
```
wdsutil /Export-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:"C:\temp\boot.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:"\\Server\Share\ExportImage.wim" /Name:"Exported WinPE image" /Description:"WinPE Image from WDS server" /Overwrite:Yes
```
若要匯出安裝映像，請輸入下列其中一項：
```
wdsutil /Export-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Filepath:"C:\Temp\Install.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:"Exported Windows image" /Description:"Windows Vista image from WDS server" /Overwrite:append
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增映像命令](using-the-add-image-command.md)
[使用 [複製影像] 命令](using-the-copy-image-command.md)
[使用 get-映像命令](using-the-get-image-command.md)
[移除映像命令](using-the-remove-image-command.md)
[使用 [取代影像] 命令](using-the-replace-image-command.md)
[子命令： 設定影像](subcommand-set-image.md)
