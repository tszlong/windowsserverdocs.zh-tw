---
title: 子命令 /start-multicasttransmission
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7e3e59a0907caf2769d5df00aeaf00589ab450d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842679"
---
# <a name="subcommand-start-multicasttransmission"></a>子命令： /start-multicasttransmission

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

啟動映像的排程傳送傳輸。
## <a name="syntax"></a>語法
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
**Windows Server 2008 R2**之開機映像：
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
安裝映像：
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定映像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
媒體類型: {安裝&#124;開機}|指定的影像類型。 請注意，此選項必須設定為**安裝**適用於 Windows Server 2008。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|若要開始傳輸相關聯的開機映像架構。 因為可能會將開機映像的相同映像名稱放在不同的架構，您應該指定以確保正確傳輸所用的架構。|
|\mediaGroup:<Image group name>]|指定映像的映像群組。 如果沒有映像群組名稱指定，且只能有一個映像群組存在於伺服器上，將會使用該映像群組。 如果伺服器上有一個以上的映像群組，您必須使用此選項來指定映像群組名稱。|
|[/ Filename:<File name>]|指定包含該映像檔的名稱。 如果映像無法依名稱來唯一識別，您必須使用此選項來指定檔案名稱。|
## <a name="BKMK_examples"></a>範例
若要啟動多點傳送的傳輸，請輸入下列其中一項：
```
wdsutil /start-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
若要啟動的開機映像的 Windows Server 2008 R2，類型的多點傳送的傳輸：
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
[使用 /get-multicasttransmission 命令](using-the-get-multicasttransmission-command.md)
[使用新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
[使用 /remove-multicasttransmission 命令](using-the-remove-multicasttransmission-command.md)
