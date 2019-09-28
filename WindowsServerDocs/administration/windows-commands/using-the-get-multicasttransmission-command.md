---
title: 使用 MulticastTransmission 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54c503abda871d1f2a4fd8a30d7f12317eee6a48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392131"
---
# <a name="using-the-get-multicasttransmission-command"></a>使用 MulticastTransmission 命令

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定映射之多播傳輸的相關資訊。
## <a name="syntax"></a>語法
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] 
[/Filename:<File name>] [/Show:Clients]
```
**Windows Server 2008 R2**開機映射傳輸：
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
    [/Server:<Server name>]
    [/details:Clients]
  mediatype:Boot
    /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
針對安裝映射傳輸：
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
         [/Server:<Server name>]
         [/details:Clients]
       mediatype:Install
    mediaGroup:<Image Group>]
     [/Filename:<File name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體： <Image name>|顯示與此映射相關聯的多播傳輸。|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體：安裝|指定映射類型。 請注意，此選項必須設定為 [**安裝**]。|
|\mediaGroup： <Image group name>]|指定包含影像的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您就必須使用這個選項來指定映射群組。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定與傳輸相關聯之開機映射的架構。 由於不同架構中的開機映射可能會有相同的映射名稱，因此您應該指定架構，以確保使用正確的映射。|
|[/Filename： <File name>]|指定包含影像的檔案。 如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。|
|[/Show：用戶端]<br /><br />或<br /><br />[/details：用戶端]|顯示連線到多播傳輸的用戶端電腦的相關資訊。|
## <a name="BKMK_examples"></a>典型
**Windows Server 2008**若要查看名為 Vista 與 Office 之映射傳輸的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2**若要查看名為 Vista 與 Office 之映射傳輸的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Office"
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)
[使用 AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
 使用[MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)，
[使用 MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md)
[子命令： start-MulticastTransmission](subcommand-start-multicasttransmission.md)
