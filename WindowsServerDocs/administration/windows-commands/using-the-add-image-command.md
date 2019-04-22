---
title: 使用新增映像命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0433e0775bd2088170ae17fcfe432cdaee0bf99d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817459"
---
# <a name="using-the-add-image-command"></a>使用新增映像命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

將影像加入至 Windows 部署服務伺服器。 如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。
## <a name="syntax"></a>語法
針對開機映像，請使用下列語法：
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] 
[/Filename:<New wim file name>]
```
安裝映像，請使用下列語法：
```
wdsutil /add-ImagmediaFile:<wim file path>
     [/Server:<Server name>]
   mediatype:Install
     [/Skipverify]
    mediaGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
mediaFile: <.wim 檔案路徑 >|指定包含要加入的映像的 Windows 映像 (.wim) 檔案的完整路徑和檔案名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未指定伺服器名稱，則會使用本機伺服器。|
mediatype:{Boot&#124;Install}|指定要加入的映像的類型。|
|[/Skipverify]|指定的完整性驗證將不會執行來源映像檔加入映像之前。|
|[/Name:<Name>]|設定映像的顯示名稱。|
|[/ 描述：<Description>]|設定映像的描述。|
|[/ Filename:<Filename>]|指定新.wim 檔案的檔案名稱。 這可讓您變更時加入映像的.wim 檔案的檔案名稱。 如果不指定任何檔案名稱，就會使用來源映像檔案名稱。 在所有情況下，Windows 部署服務會檢查以判斷在目的地電腦的開機映像存放區中是唯一的檔案名稱。|
|\mediaGroup:<Image group name>]|指定映像要加入的映像群組名稱。 如果伺服器上有一個以上的映像群組，就必須指定映像群組。 如果未指定此映像群組不存在，就會建立新的映像群組。 否則，您將使用現有的映像群組。|
|[/ SingleImage:<Single image name>] [/ 名稱：<Name>] [/ 描述：<Description>]|複製.wim 檔案中，從指定的單一映像，並設定映像的顯示名稱和描述。|
|[/ UnattendFile:<Unattend file path>]|指定要與所新增的映像相關聯的自動的安裝檔案的完整路徑。 如果 **/SingleImage**未指定，相同的自動安裝檔案會與所有.wim 檔案中的映像相關聯。|
## <a name="BKMK_examples"></a>範例
若要新增的開機映像，請輸入：
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:"My WinPE Image" 
/Description:"WinPE Image containing the WDS Client" /Filename:WDSBoot.wim
```
若要新增安裝映像，請輸入下列其中一項：
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:"Windows Pro" /Name:"My WDS Image"
/Description:"Windows Pro image with Microsoft Office" /Filename:"Win Pro.wim" /UnattendFile:"\\server\share\unattend.xml"
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 [複製影像] 命令](using-the-copy-image-command.md)
[使用 匯出映像命令](using-the-export-image-command.md)
[使用取得映像的命令](using-the-get-image-command.md)
[移除映像命令](using-the-remove-image-command.md)
[使用 [取代影像] 命令](using-the-replace-image-command.md)
 [子命令： 設定影像](subcommand-set-image.md)
