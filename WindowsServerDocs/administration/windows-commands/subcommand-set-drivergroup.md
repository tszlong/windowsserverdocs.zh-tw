---
title: 子命令集-DriverGroup
description: 子命令設定的 Windows 命令主題-DriverGroup，可設定伺服器上現有驅動程式群組的屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c49381dc65f3b2ffc9a04e4fb2699818515a931f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834031"
---
# <a name="subcommand-set-drivergroup"></a>子命令： set-DriverGroup

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定伺服器上現有驅動程式群組的屬性。

## <a name="syntax"></a>語法
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/DriverGroup：<Group Name>|指定驅動程式群組的名稱。|
|[/Server：<Server name>]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Name：<New Group Name>]|指定驅動程式群組的新名稱。|
|[/Enabled： {Yes &#124; No}|啟用或停用驅動程式群組。|
|[/Applicability： {符合&#124;全部}]|如果符合篩選準則，則指定要安裝的套件。 [**符合**] 表示只安裝符合用戶端硬體的驅動程式套件。 **All**表示不論其硬體為何，都會將所有套件安裝到用戶端。|
## <a name="examples"></a><a name=BKMK_examples></a>典型
若要設定驅動程式群組的屬性，請輸入下列其中一項：
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法索引鍵](command-line-syntax-key.md)
[子命令： set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
