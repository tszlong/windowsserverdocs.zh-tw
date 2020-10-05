---
title: 子命令集-DriverGroup
description: 子命令集的參考文章-DriverGroup，可在伺服器上設定現有驅動程式群組的屬性。
ms.topic: reference
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c4d014933ae4ebc530977325210887d4abf90817
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729856"
---
# <a name="subcommand-set-drivergroup"></a>子命令： set-DriverGroup

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定伺服器上現有驅動程式群組的屬性。

## <a name="syntax"></a>語法
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/DriverGroup:<Group Name>|指定驅動程式群組的名稱。|
|[/Server： <Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/Name： <New Group Name> ]|指定驅動程式群組的新名稱。|
|[/Enabled： {Yes &#124; 否}|啟用或停用驅動程式群組。|
|[/Applicability： {符合 &#124; All}]|指定符合篩選準則時要安裝的封裝。 [**符合**] 表示只安裝符合用戶端硬體的驅動程式套件。 **All** 表示將所有套件安裝至用戶端，不論其硬體為何。|
## <a name="examples"></a>範例
若要設定驅動程式群組的屬性，請輸入下列其中一項：
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>其他參考
- [命令列語法索引鍵](command-line-syntax-key.md) 
[子命令： set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
