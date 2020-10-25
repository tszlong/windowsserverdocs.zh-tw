---
title: 移除-DriverGroup
description: DriverGroup 的參考文章，會從伺服器移除驅動程式群組。
ms.topic: reference
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9bfec1636594f69ed9ea173cd2a0fcb95415296f
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524323"
---
# <a name="remove-drivergroup"></a>移除-DriverGroup

從伺服器移除驅動程式群組。

## <a name="syntax"></a>語法

```
wdsutil /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|/DriverGroup:\<Group Name>|指定要移除之驅動程式群組的名稱。|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|

## <a name="examples"></a>範例

若要移除驅動程式群組，請輸入下列其中一項：
```
wdsutil /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
wdsutil /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)