---
title: wdsutil 新增映射
description: Wdsutil 附加映射的參考文章，可將影像新增至 Windows 部署服務伺服器。
ms.topic: reference
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ef8068f45a8a2d49715420c06859d326bf93200a
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730010"
---
# <a name="wdsutil-add-image"></a>wdsutil 新增映射

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將影像新增至 Windows 部署服務伺服器。

## <a name="syntax"></a>Syntax
針對開機映射，請使用下列語法：
```
wdsutil /add-Image imageFile:<wim file path> [/Server:<Server name> imagetype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>]
[/Filename:<New wim file name>]
```
針對安裝映射，請使用下列語法：
```
wdsutil /add-Image imageFile:<wim file path>
     [/Server:<Server name>]
    imagetype:Install
     [/Skipverify]
     imageGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|imageFile： < .wim 檔案路徑>|指定包含要新增之映射的 Windows 映像的完整路徑和檔案名 ( .wim) 檔。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
| imagetype： {Boot&#124;Install}|指定要新增的影像類型。|
|[/Skipverify]|指定在新增映射之前，不會在來源影像檔上執行完整性驗證。|
|[/Name： <Name> ]|設定影像的顯示名稱。|
|/Description<Description>]|設定影像的描述。|
|[/Filename： <Filename> ]|指定 .wim 檔案的新檔案名。 這可讓您在新增映射時變更 .wim 檔案的檔案名。 如果未指定檔案名，將會使用來源影像檔案名稱。 在所有情況下，Windows 部署服務檢查，以判斷檔案名在目的地電腦的開機映射存放區中是否是唯一的。|
|\imageGroup： <Image group name> ]|指定要在其中新增映射的映射組名。 如果伺服器上有一個以上的映射群組，就必須指定映射群組。 如果未指定此項，而且映射群組尚未存在，則會建立新的映射群組。 否則，將會使用現有的映射群組。|
|[/SingleImage： <Single image name> ][/Name： <Name> ]/Description<Description>]|從 .wim 檔案複製指定的單一映射，並設定映射的顯示名稱和描述。|
|[/UnattendFile： <Unattend file path> ]|指定自動安裝檔案的完整路徑，以與要新增的映射產生關聯。 如果未指定 **/SingleImage** ，則相同的自動安裝檔案將會與 .wim 檔案中的所有映射相關聯。|
## <a name="examples"></a>範例
若要新增開機映射，請輸入：
```
wdsutil /add-Image imageFile:C:\MyFolder\Boot.wim imagetype:Boot
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share\Boot.wim /Server:MyWDSServer imagetype:Boot /Name:My WinPE Image
/Description:WinPE Image containing the WDS Client /Filename:WDSBoot.wim
```
若要新增安裝映射，請輸入下列其中一項：
```
wdsutil /add-Image imageFile:C:\MyFolder\Install.wim imagetype:Install
wdsutil /verbose /Progress /add-Image imageFile:\\MyServer\Share \Install.wim /Server:MyWDSServer imagetype:Instal imageGroup:ImageGroup1
/SingleImage:Windows Pro /Name:My WDS Image
/Description:Windows Pro image with Microsoft Office /Filename:Win Pro.wim /UnattendFile:\\server\share\unattend.xml
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil 複製映射命令](wdsutil-copy-image.md)
- [wdsutil 匯出-image 命令](wdsutil-export-image.md)
- [wdsutil 取得映射命令](wdsutil-get-image.md)
- [wdsutil remove-image 命令](wdsutil-remove-image.md)
- [wdsutil 取代-image 命令](wdsutil-replace-image.md)
- [wdsutil 設定-image 命令](wdsutil-set-image.md)
