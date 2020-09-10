---
title: 移除-DriverGroup
description: DriverGroup 的參考文章，會從伺服器移除驅動程式群組。
ms.topic: reference
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 55cb50d43bf433d5421f4152053844f89d45f379
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636448"
---
# <a name="remove-drivergroup"></a>移除-DriverGroup

從伺服器移除驅動程式群組。

## <a name="syntax"></a>語法

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/DriverGroup:\<Group Name>|指定要移除之驅動程式群組的名稱。|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要移除驅動程式群組，請輸入下列其中一項：
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)