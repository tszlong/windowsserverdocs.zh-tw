---
title: 子命令集-影像
description: 子命令集的參考主題-影像會變更影像的屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e24b20093a726e7553474871ef25e6877223e21f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721715"
---
# <a name="subcommand-set-image"></a>子命令：設定-影像

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更影像的屬性。

## <a name="syntax"></a>語法
開機映射：
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
媒介<Image name>|指定映像的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Boot &#124; 安裝}|指定映射的類型。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定映射的架構。 因為不同架構中的不同開機映射可以有相同的映射名稱，所以指定架構可確保修改了正確的映射。|
|[/Filename：<File name>]|如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。|
|/Name|指定映像的名稱。|
|/Description<Description>]|設定影像的描述。|
|[/Enabled： {Yes &#124; No}]|啟用或停用映射。|
|\mediaGroup：<Image group name>]|指定包含影像的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您就必須使用這個選項來指定映射群組。|
|[/UserFilter：<SDDL>]|設定影像上的使用者篩選。 篩選字串必須是安全描述項定義語言（SDDL）格式。 請注意，與映射群組的 **/Security**選項不同的是，這個選項只會限制誰可以看到影像定義，而不是實際的影像檔案資源。 若要限制檔案資源的存取權，以及存取映射群組中的所有映射，您必須為映射群組本身設定安全性。|
|[/UnattendFile：<Unattend file path>]|設定要與映射相關聯之自動安裝檔案的完整路徑。 例如： **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend： {Yes &#124; No}]|如果已有與映射相關聯的自動安裝檔案，您可以指定 **/Overwrite**來覆寫自動安裝檔案。 請注意，預設設定為 [**否**]。|
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
- [命令列語法索引鍵](command-line-syntax-key.md)
[Using the add-Image Command](using-the-add-image-command.md)
：使用使用 [映射] 命令的 [[複製映射](using-the-copy-image-command.md)
[Using the Export-Image Command](using-the-export-image-command.md)
[Using the get-Image Command](using-the-get-image-command.md)
[Using the remove-Image Command](using-the-remove-image-command.md)
] 命令，使用 [映射] 命令，並使用 [[取代映射](using-the-replace-image-command.md)] 命令使用 [映射] 命令
