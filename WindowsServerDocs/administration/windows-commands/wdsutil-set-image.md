---
title: wdsutil 設定-影像
description: Wdsutil 設定影像的參考文章，可變更影像的屬性。
ms.topic: reference
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fa85c4500475cf9f6e184d0ed0f3b3d6e4dc51aa
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730602"
---
# <a name="wdsutil-set-image"></a>wdsutil 設定-影像

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更影像的屬性。

## <a name="syntax"></a>Syntax
針對開機映射：
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>]
[/Description:<Description>] [/Enabled:{Yes | No}]
```
針對安裝映射：
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     [/Name:<Name>]
     [/Description:<Description>]
     [/UserFilter:<SDDL>]
     [/Enabled:{Yes | No}]
     [/UnattendFile:<Unattend file path>]
         [/OverwriteUnattend:{Yes | No}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定映像的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體媒體： {Boot &#124; Install}|指定映射的類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定映射的架構。 由於您可以在不同的架構中，針對不同的開機映射擁有相同的映射名稱，因此指定架構可確保修改正確的映射。|
|[/Filename： <File name> ]|如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
|/Name|指定映像的名稱。|
|/Description<Description>]|設定影像的描述。|
|[/Enabled： {Yes &#124; No}]|啟用或停用映射。|
|\mediaGroup： <Image group name> ]|指定包含映射的映射群組。 如果未指定映射組名，且伺服器上只有一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您必須使用此選項來指定映射群組。|
|[/UserFilter： <SDDL> ]|設定影像的使用者篩選。 篩選字串必須是安全描述項定義語言 (SDDL) 格式。 請注意，與映射群組的 **/Security** 選項不同的是，此選項只會限制誰可以看到影像定義，而不是實際的影像檔案資源。 若要限制對檔案資源的存取，進而存取映射群組內的所有映射，您必須設定映射群組本身的安全性。|
|[/UnattendFile： <Unattend file path> ]|設定要與映射相關聯的自動安裝檔案的完整路徑。 例如： **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend： {Yes &#124; No}]|如果已有與映射相關聯的自動安裝檔案，您可以指定 **/Overwrite** 覆寫自動安裝檔案。 請注意，預設設定為 [ **否**]。|
## <a name="examples"></a>範例
若要設定開機映射的值，請輸入下列其中一項：
```
wdsutil /Set-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /Description:New description
wdsutil /verbose /Set-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
/Name:New Name /Description:New Description /Enabled:Yes
```
若要設定安裝映射的值，請輸入下列其中一項：
```
wdsutil /Set-Imagmedia:Windows Vista with Officemediatype:Install /Description:New description
wdsutil /verbose /Set-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /Name:New name /Description:New description /UserFilter:O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU) /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil add-image 命令](wdsutil-add-image.md)
- [wdsutil 複製映射命令](wdsutil-copy-image.md)
- [wdsutil 匯出-image 命令](wdsutil-export-image.md)
- [wdsutil 取得映射命令](wdsutil-get-image.md)
- [wdsutil remove-image 命令](wdsutil-remove-image.md)
- [wdsutil 取代-image 命令](wdsutil-replace-image.md)
