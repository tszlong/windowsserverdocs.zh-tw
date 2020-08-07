---
title: 新增-MulticastTransmission
description: MulticastTransmission 的參考文章，可為映射建立新的多播傳輸。
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 77f25940a3316d715bced365e92ed0614c74ed33
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896901"
---
# <a name="new-multicasttransmission"></a>新增-MulticastTransmission

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為映射建立新的多播傳輸。 此命令等同于使用 Windows 部署服務 mmc 嵌入式管理單元來建立傳輸 (以滑鼠右鍵按一下 [**多播傳輸**] 節點，然後按一下 [**建立多播傳輸**) ]。 當您已安裝部署伺服器角色服務和傳輸伺服器角色服務時，應該使用此命令 (這是預設的安裝) 。 如果您只安裝了「傳輸伺服器」角色服務，請使用[新的-Namespace 命令](using-the-new-namespace-command.md)。
## <a name="syntax"></a>語法
針對安裝映射傳輸：
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
針對開機映射傳輸 (僅支援 Windows Server 2008 R2) ：
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
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
媒介<Image name>|指定要使用多播來傳輸的映射名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 (FQDN) 的完整功能變數名稱。 如果未指定伺服器名稱，則會使用本機伺服器。|
|FriendlyName<Friendly name>|指定傳輸的易記名稱。|
|/Description<Description>]|指定傳輸的描述。|
媒體： {Boot&#124;安裝}|指定要使用多播來傳輸的映射類型。 請注意，只有 Windows Server 2008 R2 支援**開機**。|
|\mediaGroup： <Image group name> ]|指定包含影像的映射群組。 如果未指定映射組名，且伺服器上只存在一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您就必須使用這個選項來指定映射組名。|
|[/Filename： <File name> ]|指定檔案名稱。 如果來源映射無法以名稱唯一識別，您就必須使用這個選項來指定檔案名。|
|/Transmissiontype： {AutoCast &#124; ScheduledCast}|指定是否要根據指定的開始準則，自動啟動傳輸 (AutoCast) 或 (ScheduledCast) 。<p><ul><li>**自動轉換**。 此傳輸類型表示只要有適用的用戶端要求安裝映射，所選映射的多播傳輸就會開始。 當其他用戶端要求相同的映射時，它們會聯結到已啟動的傳輸。</li><li>已**排程-轉換**。 此傳輸類型會根據要求映射的用戶端數目和/或特定的日期和時間，設定傳輸的開始準則。 您可以指定下列選項：<p><ul><li>[/time： <time> ]-使用下列格式設定傳輸的開始時間： YYYY/MM/DD： hh： MM。</li><li>[/Clients： <Number of clients> ]-設定要在傳輸開始前等待的最小用戶端數目。</li></ul></li></ul>|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定要使用多播來傳輸之開機映射的架構。 因為不同架構的開機映射可能會有相同的名稱，所以您應該指定架構，以確保使用正確的映射。|
|[/Filename： <File name> ]|指定檔案名稱。 如果來源映射無法以名稱唯一識別，您就必須指定檔案名。|
## <a name="examples"></a>範例
若要在 Windows Server 2008 R2 中建立自動轉換的開機映射傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS Boot Transmission
/Image:X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
若要建立自動轉換的安裝映射傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS AutoCast Transmission
/Image:Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
若要建立安裝映射的排程轉換傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS SchedCast Transmission /Server:MyWDSServemedia:Vista with Officemediatype:Install
/Transmissiontype:ScheduledCast /time:2006/11/20:17:00 /Clients:100
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md) 
[使用 AllMulticastTransmissions 命令](using-the-get-allmulticasttransmissions-command.md) 
[使用 MulticastTransmission 命令](using-the-get-multicasttransmission-command.md) 
[使用 MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md) 
[子命令： start-MulticastTransmission](subcommand-start-multicasttransmission.md)
