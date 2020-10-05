---
title: wdsutil get-multicasttransmission
description: Wdsutil multicasttransmission 的參考文章，其中顯示指定映射之多播傳輸的相關資訊。
ms.topic: reference
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ceb31ea1d1d9a266c7b8ab13a859275140b98317
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729892"
---
# <a name="wdsutil-get-multicasttransmission"></a>wdsutil get-multicasttransmission

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定映射之多播傳輸的相關資訊。

## <a name="syntax"></a>Syntax
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>]
[/Filename:<File name>] [/Show:Clients]
```
適用于開機映射傳輸的**Windows Server 2008 R2** ：
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
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|顯示與此映射相關聯的多播傳輸。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|/imagetype：安裝|指定影像類型。 請注意，此選項必須設定為 [ **安裝**]。|
|/imagegroup： <Image group name> ]|指定包含映射的映射群組。 如果未指定映射組名，且伺服器上只有一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您必須使用此選項來指定映射群組。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定與傳輸相關聯之開機映射的架構。 由於不同架構中的開機映射可以有相同的映射名稱，因此您應該指定架構，以確保使用正確的映射。|
|[/Filename： <File name> ]|指定包含映射的檔案。 如果映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
|[/Show：用戶端]<p>或<p>[/details：用戶端]|顯示連線到多播傳輸之用戶端電腦的相關資訊。|
## <a name="examples"></a>範例
**Windows Server 2008** 若要使用 Office 來查看名為 Vista 之映射的傳輸相關資訊，請輸入下列其中一項：
```
wdsutil /Get-MulticastTransmission:Vista with Office imagetype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServer image:Vista with Office imagetype:Install imageGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** 若要使用 Office 來查看名為 Vista 之映射的傳輸相關資訊，請輸入下列其中一項：
```
wdsutil /Get-MulticastTransmission:Vista with Office
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServer image:Vista with Office imagetype:Install ImageGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServer:X64 Boot Imagetype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allmulticasttransmissions 命令](wdsutil-get-allmulticasttransmissions.md)
- [wdsutil new-multicasttransmission 命令](wdsutil-new-multicasttransmission.md)
- [wdsutil remove-multicasttransmission 命令](wdsutil-remove-multicasttransmission.md)
- [wdsutil 開始-multicasttransmission 命令](wdsutil-start-multicasttransmission.md)