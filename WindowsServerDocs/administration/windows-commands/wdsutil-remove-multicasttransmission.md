---
title: wdsutil 移除-multicasttransmission
description: Wdsutil multicasttransmission 的參考文章，可停用影像的多播傳輸。
ms.topic: reference
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a7ed8d9ef26deb0a50baa8511cd07f250561d579
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730596"
---
# <a name="wdsutil-remove-multicasttransmission"></a>wdsutil 移除-multicasttransmission

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停用映射的多播傳輸。 除非您指定 **/force**，否則現有的用戶端將會完成映射傳送，但不允許新的用戶端加入。

## <a name="syntax"></a>Syntax
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmission:<Image name> [/Server:<Server name> mediatype:Install Group:<Image Group>] [/Filename:<File name>] [/force]
```
適用于開機映射的**Windows Server 2008 R2** ：
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
針對安裝映射：
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|媒體：<Image name>|指定映像的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體媒體： {安裝&#124;Boot}|指定影像類型。 請注意，此選項必須設定為 Windows Server 2008 的 [ **安裝** ]。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定與要啟動的傳輸相關聯之開機映射的架構。 由於不同架構中的開機映射可以有相同的映射名稱，因此您應該指定架構，以確保使用正確的傳輸。|
|\mediaGroup： <Image group name> ]|指定包含映射的映射群組。 如果未指定映射組名，且伺服器上只有一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您必須使用此選項來指定映射組名。|
|[/Filename： <File name> ]|指定檔案名稱。 如果來源映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
|/force|移除傳輸並終止所有用戶端。 除非您指定 **/force** 選項的值，否則現有的用戶端可以完成映射傳送，但無法加入新的用戶端。|
## <a name="examples"></a>範例
若要停止命名空間 (目前的用戶端會完成傳輸，但新的用戶端將無法加入) ，請輸入：
```
wdsutil /remove-MulticastTransmission:Vista with Office
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmission:x64 Boot Image
/Imagetype:Boot /Architecture:x64
```
若要強制終止所有用戶端，請輸入：
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allmulticasttransmissions 命令](wdsutil-get-allmulticasttransmissions.md)
- [wdsutil get-multicasttransmission 命令](wdsutil-get-multicasttransmission.md)
- [wdsutil new-multicasttransmission 命令](wdsutil-new-multicasttransmission.md)
- [wdsutil 開始-multicasttransmission 命令](wdsutil-start-multicasttransmission.md)
