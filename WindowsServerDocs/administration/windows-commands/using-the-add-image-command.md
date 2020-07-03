---
title: 新增-映射
description: 新增映射的參考文章，它會將影像新增至 Windows 部署服務伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 535c303e779441dd164174e7a7e311747a9c1e4d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929138"
---
# <a name="add-image"></a>新增-映射

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將影像新增至 Windows 部署服務伺服器。

## <a name="syntax"></a>Syntax
針對開機映射，請使用下列語法：
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>]
[/Filename:<New wim file name>]
```
針對安裝映射，請使用下列語法：
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
### <a name="parameters"></a>參數
|參數|說明|
|-------|--------|
mediaFile： < .wim 檔案路徑>|指定包含要加入之映射的 Windows 映像（.wim）檔案的完整路徑和檔案名。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Boot&#124;安裝}|指定要加入之映射的類型。|
|[/Skipverify]|指定在加入映射之前，不會在來源影像檔上執行完整性驗證。|
|[/Name： <Name> ]|設定影像的顯示名稱。|
|/Description<Description>]|設定影像的描述。|
|[/Filename： <Filename> ]|為 .wim 檔案指定新的檔案名。 這可讓您在新增映射時，變更 .wim 檔案的檔案名。 如果未指定檔案名，則會使用來源影像檔案名稱。 在所有情況下，Windows 部署服務檢查以判斷檔案名在目的地電腦的開機映射存放區中是否是唯一的。|
|\mediaGroup： <Image group name> ]|指定要在其中新增影像之映射群組的名稱。 如果伺服器上有一個以上的映射群組，則必須指定映射群組。 如果未指定此功能，而且映射群組尚不存在，則會建立新的映射群組。 否則，將會使用現有的映射群組。|
|[/SingleImage： <Single image name> ][/Name： <Name> ]/Description<Description>]|將指定的單一映射複製到 .wim 檔案中，並設定映射的顯示名稱和描述。|
|[/UnattendFile： <Unattend file path> ]|指定要與新增之映射相關聯的自動安裝檔案的完整路徑。 如果未指定 **/SingleImage** ，則相同的自動安裝檔案會與 .wim 檔案中的所有映射產生關聯。|
## <a name="examples"></a>範例
若要新增開機映射，請輸入：
```
wdsutil /add-ImagmediaFile:C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:My WinPE Image
/Description:WinPE Image containing the WDS Client /Filename:WDSBoot.wim
```
若要新增安裝映射，請輸入下列其中一項：
```
wdsutil /add-ImagmediaFile:C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/SingleImage:Windows Pro /Name:My WDS Image
/Description:Windows Pro image with Microsoft Office /Filename:Win Pro.wim /UnattendFile:\\server\share\unattend.xml
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用複製映射命令](using-the-copy-image-command.md) 
[使用匯出-映射命令](using-the-export-image-command.md) 
[使用取得映射命令](using-the-get-image-command.md) 
[使用移除映射命令](using-the-remove-image-command.md) 
[使用取代映射命令](using-the-replace-image-command.md) 
[子命令：設定-影像](subcommand-set-image.md)
