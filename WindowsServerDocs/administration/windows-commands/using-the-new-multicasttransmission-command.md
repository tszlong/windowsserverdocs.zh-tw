---
title: 使用新 MulticastTransmission 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84f5aa69f2d4a875995ac6c18fa43bd68518fba3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875139"
---
# <a name="using-the-new-multicasttransmission-command"></a>使用新 MulticastTransmission 命令

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

建立映像的新多點傳送的傳輸。 此命令等同於使用 Windows 部署服務 mmc 嵌入式管理單元建立的傳輸 (以滑鼠右鍵按一下**多點傳送傳輸**節點，然後再按一下**建立多點傳送傳輸**). 您已經部署伺服器角色服務和傳輸伺服器角色服務安裝 （這是預設安裝） 時，您應該使用此命令。 如果您只傳輸伺服器角色服務安裝，請使用[使用新命名空間命令](using-the-new-namespace-command.md)。
## <a name="syntax"></a>語法
安裝映像傳輸：
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
針對開機映像傳輸 （僅支援 Windows Server 2008 R2）：
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒體：<Image name>|指定要使用多點傳送傳輸的映像的名稱。|
|[/ 伺服器：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整的網域名稱 (FQDN)。 如果未不指定任何伺服器名稱，則會使用本機伺服器。|
|/Friendlyname:<Friendly name>|指定傳輸的易記名稱。|
|[/ 描述：<Description>]|指定傳輸的描述。|
mediatype:{Boot&#124;Install}|指定要使用多點傳送傳輸的映像的類型。 附註**開機**僅適用於 Windows Server 2008 r2。|
|\mediaGroup:<Image group name>]|指定包含該映像的映像群組。 如果沒有映像群組名稱指定，且只能有一個映像群組存在於伺服器上，則會使用該映像群組。 如果伺服器上有一個以上的映像群組，您必須使用此選項來指定映像群組名稱。|
|[/ Filename:<File name>]|指定的檔案名稱。 如果名稱不能唯一識別來源映像，您必須使用此選項來指定檔案名稱。|
|/Transmissiontype:{AutoCast &#124; ScheduledCast}|指定是否要自動啟動的傳輸 (AutoCast) 或根據指定的開始條件 （排程傳送）。<br /><br /><ul><li>**自動傳送**。 這個傳輸型別表示，適用的用戶端要求安裝映像，當選取的映像的多點傳送的傳輸就會開始。 當其他用戶端要求相同的映像，他們會加入已經啟動的傳輸。</li><li>**排程傳送**。 此傳輸類型設定的映像及/或特定的日期和時間所要求的用戶端數目為基礎的傳輸的開始條件。 您可以指定下列選項：<br /><br /><ul><li>[/time: <time>]-設定傳輸應使用下列格式啟動的時間：YYYY/MM/y。</li><li>[/ 用戶端： <Number of clients>]-設定用戶端傳輸啟動之前要等待的最小數目。</li></ul></li></ul>|
|/ 架構: {x86 &#124; ia64 &#124; x64}|指定傳輸使用多點傳送的開機映像的架構。 因為它可能會有相同名稱的不同架構的開機映像，您應該指定以確保使用正確的映像的架構。|
|[/ Filename:<File name>]|指定的檔案名稱。 如果名稱不能唯一識別來源映像，您必須指定檔案名稱。|
## <a name="BKMK_examples"></a>範例
若要建立 Windows Server 2008 R2 中的開機映像自動傳送 」 傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS Boot Transmission"
/Image:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
若要建立的安裝映像自動傳送 」 傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS AutoCast Transmission"
/Image:"Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
若要建立安裝映像的排程傳送 」 傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS SchedCast Transmission" /Server:MyWDSServemedia:"Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:"2006/11/20:17:00" /Clients:100
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法重點](command-line-syntax-key.md)
[使用 get AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md)
[使用 /get-multicasttransmission 命令](using-the-get-multicasttransmission-command.md)
[使用 /remove-multicasttransmission 命令](using-the-remove-multicasttransmission-command.md)
[子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
