---
title: 子命令開始-MulticastTransmission
description: 命令 MulticastTransmission 的 Windows 命令主題，它會啟動映射的排程轉換傳輸。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e882c54d2fbe744ca9fe25b2631f4d875886c756
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833781"
---
# <a name="subcommand-start-multicasttransmission"></a>子命令： start-MulticastTransmission

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

啟動映射的排程轉換傳輸。

## <a name="syntax"></a>語法
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
適用于開機映射的**Windows Server 2008 R2** ：
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
針對安裝映射：
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定映像的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Install&#124;Boot}|指定映射類型。 請注意，此選項必須設定為 [針對 Windows Server 2008**安裝**]。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|與要啟動的傳輸相關聯之開機映射的架構。 由於不同架構中的開機映射可能會有相同的映射名稱，因此您應該指定架構，以確保使用正確的傳輸。|
|\mediaGroup：<Image group name>]|指定映射的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您就必須使用這個選項來指定映射組名。|
|[/Filename：<File name>]|指定包含影像的檔案名。 如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要啟動多播傳輸，請輸入下列其中一項：
```
wdsutil /start-MulticastTransmissiomedia:Vista with Office
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
若要啟動 Windows Server 2008 R2 的開機映射多播傳輸，請輸入：
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
[使用 AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
使用[MulticastTransmission 命令](using-the-get-multicasttransmission-command.md)
使用[MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
[使用 remove-MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md)
