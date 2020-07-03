---
title: 移除-MulticastTransmission
description: MulticastTransmission 的參考文章，這會停用映射的多播傳輸。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5f695e4743b06eb8a2e1c59081a4661e616c8711
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931236"
---
# <a name="using-the-remove-multicasttransmission-command"></a>使用 MulticastTransmission 命令

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

停用映射的多播傳輸。 除非您指定 **/force**，否則現有的用戶端將會完成映射傳輸，但不會允許新的用戶端加入。

## <a name="syntax"></a>Syntax
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
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
|參數|說明|
|-------|--------|
媒介<Image name>|指定映像的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Install&#124;Boot}|指定映射類型。 請注意，此選項必須設定為 [針對 Windows Server 2008**安裝**]。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定與要啟動之傳輸相關聯之開機映射的架構。 由於不同架構中的開機映射可能會有相同的映射名稱，因此您應該指定架構，以確保使用正確的傳輸。|
|\mediaGroup： <Image group name> ]|指定包含影像的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您就必須使用這個選項來指定映射組名。|
|[/Filename： <File name> ]|指定檔案名稱。 如果來源映射無法以名稱唯一識別，您就必須使用這個選項來指定檔案名。|
|/force|移除傳輸並終止所有用戶端。 除非您指定 **/force**選項的值，否則現有的用戶端可以完成映射傳輸，但新的用戶端無法加入。|
## <a name="examples"></a>範例
若要停止命名空間（目前的用戶端將會完成傳輸，但新的用戶端將無法加入），請輸入：
```
wdsutil /remove-MulticastTransmissiomedia:Vista with Office
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:x64 Boot Image
/Imagetype:Boot /Architecture:x64
```
若要強制終止所有用戶端，請輸入：
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md) 
[使用 MulticastTransmission 命令](using-the-get-multicasttransmission-command.md) 
[使用 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md) 
[子命令： start-MulticastTransmission](subcommand-start-multicasttransmission.md)
