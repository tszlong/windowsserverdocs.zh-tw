---
title: wdsutil new-multicasttransmission
description: Wdsutil new-multicasttransmission 的參考文章，會為映射建立新的多播傳輸。
ms.topic: reference
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4b02668a6e714ce090a013a34d9dee73121f7a89
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729873"
---
# <a name="wdsutil-new-multicasttransmission"></a>wdsutil new-multicasttransmission

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

為映射建立新的多播傳輸。 此命令相當於使用 Windows 部署服務 mmc 嵌入式管理單元來建立傳輸 (以滑鼠右鍵按一下 [ **多播傳輸** ] 節點，然後按一下 [ **建立多播傳輸** ]) 。 當您安裝了部署伺服器角色服務和傳輸伺服器角色服務時，應該使用這個命令 (這是預設的安裝) 。 如果您只安裝傳輸伺服器角色服務，請使用 [wdsutilnew-Namespace 命令](wdsutil-new-namespace.md)。
## <a name="syntax"></a>Syntax
針對安裝映射傳輸：
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      imagetype:Install
       ImageGroup:<Image Group>]
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
      imagetype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
| /image<Image name>|指定要使用多播傳輸的映射名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或完整功能變數名稱 (FQDN) 。 如果未指定伺服器名稱，則會使用本機伺服器。|
|友好<Friendly name>|指定傳輸的易記名稱。|
|/Description<Description>]|指定傳輸的描述。|
| /imagetype： {Boot&#124;Install}|指定要使用多播傳輸的影像類型。 請注意，只有 Windows Server 2008 R2 支援 **開機** 。|
| /ImageGroup： <Image group name> ]|指定包含映射的映射群組。 如果未指定映射組名，且伺服器上只有一個映射群組，則會使用該映射群組。 如果伺服器上有一個以上的映射群組，您必須使用此選項來指定映射組名。|
|[/Filename： <File name> ]|指定檔案名稱。 如果來源映射無法依名稱唯一識別，您必須使用此選項來指定檔案名。|
|/Transmissiontype： {AutoCast &#124; ScheduledCast}|指定是否要 (AutoCast) 或根據指定的起始準則 (ScheduledCast) ，自動啟動傳輸。<p><ul><li>**自動轉換**。 此傳輸類型指出，一旦適用的用戶端要求安裝映射時，所選映射的多播傳輸就會開始。 當其他用戶端要求相同的映射時，它們就會聯結至已啟動的傳輸。</li><li>已**排程的轉換**。 此傳輸類型會根據要求映射的用戶端數目和/或特定的日期和時間，設定傳輸的開始準則。 您可以指定下列選項：<p><ul><li>[/time： <time> ]-使用下列格式設定傳輸應開始的時間： YYYY/MM/DD： hh： MM。</li><li>[/Clients： <Number of clients> ]-設定開始傳輸之前等候的最小用戶端數目。</li></ul></li></ul>|
|/Architecture： {x86 &#124; ia64 &#124; x64}|指定要使用多播傳輸的開機映射架構。 因為不同架構的開機映射可以有相同的名稱，所以您應該指定架構，以確保使用正確的映射。|
|[/Filename： <File name> ]|指定檔案名稱。 如果來源映射無法依名稱唯一識別，您必須指定檔案名。|
## <a name="examples"></a>範例
若要在 Windows Server 2008 R2 中建立開機映射的自動轉換傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS Boot Transmission
/Image:X64 Boot imagetype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
若要建立安裝映射的自動轉換傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS AutoCast Transmission
/Image:Vista with Officeimage imagetype:Install /Transmissiontype:AutoCast
```
若要建立安裝映射的排程轉換傳輸，請輸入：
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS SchedCast Transmission /Server:MyWDSServer Image:Vista with Office imagetype:Install
/Transmissiontype:ScheduledCast /time:2006/11/20:17:00 /Clients:100
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil get-allmulticasttransmissions 命令](wdsutil-get-allmulticasttransmissions.md)
- [wdsutil get-multicasttransmission 命令](wdsutil-get-multicasttransmission.md)
- [wdsutil remove-multicasttransmission 命令](wdsutil-remove-multicasttransmission.md)
- [wdsutil 開始-multicasttransmission 命令](wdsutil-start-multicasttransmission.md)
