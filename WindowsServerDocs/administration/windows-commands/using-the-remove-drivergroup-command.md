---
title: 移除-DriverGroup
description: DriverGroup 的參考文章，會從伺服器移除驅動程式群組。
ms.topic: reference
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2e0112ab1b85f37d148cb3f2b1c26b217b1047d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038106"
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