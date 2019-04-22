---
title: 使用 /get-multicasttransmission 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fdbb9283285bcf56cd83c18ea076e3d36a51b966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823229"
---
# <a name="using-the-get-multicasttransmission-command"></a>使用 /get-multicasttransmission 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

顯示指定的映像的多點傳送傳輸的相關資訊。
## <a name="syntax"></a>語法
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] 
[/Filename:<File name>] [/Show:Clients]
```
**Windows Server 2008 R2** ，將開機映像傳輸：
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
    [/Server:<Server name>]
    [/details:Clients]
  mediatype:Boot
    /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
安裝映像傳輸：
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
媒體：<Image name>|顯示此映像相關聯的多點傳送的傳輸。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
mediatype:Install|指定的影像類型。 請注意，此選項必須設定為**安裝**。|
|\mediaGroup:<Image group name>]|指定包含該映像的映像群組。 如果沒有映像群組名稱指定，且只能有一個映像群組存在於伺服器上，則會使用該映像群組。 如果伺服器上有一個以上的映像群組，您必須使用此選項來指定映像群組。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定與傳輸相關聯的開機映像的架構。 因為它可能會有將開機映像的相同映像名稱放在不同的架構，您應該指定要確保使用正確的映像的架構。|
|[/ Filename:<File name>]|指定包含該映像的檔案。 如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。|
|[/Show:Clients]<br /><br />或<br /><br />[/details:Clients]|顯示用戶端電腦連線到多點傳送傳輸的相關資訊。|
## <a name="BKMK_examples"></a>範例
**Windows Server 2008**若要檢視名為 Vista 與 Office 映像傳輸的資訊，請輸入下列其中之一：
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2**若要檢視名為 Vista 與 Office 映像傳輸的資訊，請輸入下列其中之一：
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
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
[使用 新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
[使用 /remove-multicasttransmission 命令](using-the-remove-multicasttransmission-command.md)
[子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
