---
title: 使用 /remove-multicasttransmission 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc3ba385644ef9da9b5d592142091ff087cd7545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839679"
---
# <a name="using-the-remove-multicasttransmission-command"></a>使用 /remove-multicasttransmission 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

停用映像的多點傳送傳輸。 除非您指定 **/force**，現有的用戶端將會完成映像傳輸，但不是會允許新用戶端加入。
## <a name="syntax"></a>語法
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2**之開機映像：
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
安裝映像：
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定映像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果沒有指定伺服器名稱時，會使用本機伺服器。|
媒體類型: {安裝&#124;開機}|指定的影像類型。 請注意，此選項必須設定為**安裝**適用於 Windows Server 2008。|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定開始傳輸相關聯的開機映像的架構。 因為它可能會有將開機映像的相同映像名稱放在不同的架構，您應該指定以確保正確傳輸所用的架構。|
|\mediaGroup:<Image group name>]|指定包含該映像的映像群組。 如果沒有映像群組名稱指定，且只能有一個映像群組存在於伺服器上，則會使用該映像群組。 如果伺服器上有一個以上的映像群組，您必須使用此選項來指定映像群組名稱。|
|[/ Filename:<File name>]|指定的檔案名稱。 如果名稱不能唯一識別來源映像，您必須使用此選項來指定檔案名稱。|
|[/force]|移除傳輸，並會終止所有用戶端。 除非您指定的值，否則 **/force**選項時，現有的用戶端可以完成映像傳輸，但不能加入了新的用戶端。|
## <a name="BKMK_examples"></a>範例
若要停止的命名空間 （目前的用戶端將會完成傳輸，但將無法再加入新的用戶端），型別：
```
wdsutil /remove-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:"x64 Boot Image"
/Imagetype:Boot /Architecture:x64
```
若要強制終止所有用戶端，請輸入：
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
[使用 /get-multicasttransmission 命令](using-the-get-multicasttransmission-command.md)
[使用新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
[子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
