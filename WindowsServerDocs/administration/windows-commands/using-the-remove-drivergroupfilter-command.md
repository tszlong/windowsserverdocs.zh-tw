---
title: 移除-DriverGroupFilter
description: DriverGroupFilter 的參考文章，它會從伺服器上的驅動程式群組移除篩選規則。
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 902dd41e7efbb8d7821737d4555a3e5ef957a17a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896362"
---
# <a name="remove-drivergroupfilter"></a>移除-DriverGroupFilter



從伺服器上的驅動程式群組移除篩選規則。

## <a name="syntax"></a>語法

```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/DriverGroup:\<Group Name>|指定驅動程式群組的名稱。|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/FilterType： \<FilterType> ]|指定要從群組中移除的篩選準則類型。 \<FilterType>可以是下列其中一項：</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**製造商**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="examples"></a>範例

若要移除篩選，請輸入下列其中一項：
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)