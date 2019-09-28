---
title: 子命令開始-MulticastTransmission
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c0e05a1d625e560d85f0af6ae1d76ef8116ddfd8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383834"
---
# <a name="subcommand-start-multicasttransmission"></a>子命令： start-MulticastTransmission

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

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
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體： <Image name>|指定映射的名稱。|
|[/Server： <Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱（FQDN）。 如果未指定伺服器名稱，則會使用本機伺服器。|
媒體： {Install&#124;Boot}|指定映射類型。 請注意，此選項必須設定為 [針對 Windows Server 2008**安裝**]。|
|/Architecture： {x86 &#124; ia64 &#124; x64}|與要啟動的傳輸相關聯之開機映射的架構。 由於不同架構中的開機映射可能會有相同的映射名稱，因此您應該指定架構，以確保使用正確的傳輸。|
|\mediaGroup： <Image group name>]|指定映射的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您就必須使用這個選項來指定映射組名。|
|[/Filename： <File name>]|指定包含影像的檔案名。 如果無法以名稱唯一識別映射，您就必須使用這個選項來指定檔案名。|
## <a name="BKMK_examples"></a>典型
若要啟動多播傳輸，請輸入下列其中一項：
```
wdsutil /start-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
若要啟動 Windows Server 2008 R2 的開機映射多播傳輸，請輸入：
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
#### <a name="additional-references"></a>其他參考
[命令列語法索引鍵](command-line-syntax-key.md)@no__t-@no__t[1 使用](using-the-get-allmulticasttransmissions-command.md)[MulticastTransmission 命令](using-the-get-multicasttransmission-command.md)，請在-3 中使用[MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)，
[，並使用 
MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md)
