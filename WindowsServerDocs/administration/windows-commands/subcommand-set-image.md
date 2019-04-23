---
title: 子命令設定影像
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ae03c86-7a13-4e38-9182-32e55fffd504
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e63c67210764de76edae18a1897a68d763f9d695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856479"
---
# <a name="subcommand-set-image"></a>子命令： 設定影像

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更影像的屬性。
## <a name="syntax"></a>語法
針對開機映像：
```
wdsutil /Set-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>] [/Name:<Name>] 
[/Description:<Description>] [/Enabled:{Yes | No}]
```
安裝映像：
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
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定映像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
媒體類型: {開機&#124;安裝}|指定映像的類型。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定映像的架構。 您可以在不同的架構不同的開機映像的相同映像名稱，因為指定的架構可確保正確的映像會修改。|
|[/ Filename:<File name>]|如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。|
|[/Name]|指定映像的名稱。|
|[/ 描述：<Description>]|設定映像的描述。|
|[/Enabled: {[是] &#124; No}]|啟用或停用映像。|
|\mediaGroup:<Image group name>]|指定包含該映像的映像群組。 如果沒有映像群組名稱指定，且只能有一個映像群組存在於伺服器上，將會使用該映像群組。 如果伺服器上有一個以上的映像群組，您必須使用此選項來指定映像群組。|
|[/ UserFilter:<SDDL>]|映像上設定使用者篩選器。 篩選條件字串必須以 Security Descriptor Definition Language (SDDL) 格式。 請注意，不同於 **/Security**選項的映像群組，此選項只會限制誰可以查看映像定義，並不實際的影像檔資源。 若要限制存取的檔案資源，並因此存取映像群組內的所有映像，您必須設定映像群組本身的安全性。|
|[/ UnattendFile:<Unattend file path>]|設定自動安裝檔案與影像相關聯的完整路徑。 例如: **D:\Files\Unattend\Img1Unattend.xml**|
|[/OverwriteUnattend:{Yes &#124; No}]|您可以指定**覆寫/** 覆寫自動安裝檔案，如果已經有相關聯的映像自動安裝檔案。 請注意，預設值是**No**。|
## <a name="BKMK_examples"></a>範例
若要設定開機映像的值，請輸入下列其中一項：
```
wdsutil /Set-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /Description:"New description"
wdsutil /verbose /Set-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim 
/Name:"New Name" /Description:"New Description" /Enabled:Yes
```
若要設定為安裝映像的值，請輸入下列其中一項：
```
wdsutil /Set-Imagmedia:"Windows Vista with Officemediatype:Install /Description:"New description" 
wdsutil /verbose /Set-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /Name:"New name" /Description:"New description" /UserFilter:"O:BAG:DUD:AI(A;ID;FA;;;SY)(A;ID;FA;;;BA)(A;ID;0x1200a9;;;AU)" /Enabled:Yes /UnattendFile:\\server\share\unattend.xml /OverwriteUnattend:Yes
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用新增映像命令](using-the-add-image-command.md)
[使用 [複製影像] 命令](using-the-copy-image-command.md)
[使用匯出映像命令](using-the-export-image-command.md)
[取得映像命令](using-the-get-image-command.md)
[移除映像命令](using-the-remove-image-command.md)
 [使用 [取代影像] 命令](using-the-replace-image-command.md)
