---
title: 移除-DriverGroupPackage
description: DriverGroupPackage 的參考文章，它會從伺服器上的驅動程式群組移除驅動程式套件。
ms.topic: reference
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: af8354cfce9729e459da695dd2ac203d3c4e6891
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524313"
---
# <a name="remove-drivergrouppackage"></a>移除-DriverGroupPackage



從伺服器上的驅動程式群組移除驅動程式套件。

## <a name="syntax"></a>語法

```
wdsutil /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/DriverPackage： \<Name> ]|指定要移除之驅動程式套件的名稱。|
|[/PackageId： \<ID> ]|指定要移除之驅動程式套件的 Windows 部署服務識別碼。 如果您無法依名稱唯一識別驅動程式套件，則必須指定此選項。|

## <a name="examples"></a>範例

```
wdsutil /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
wdsutil /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)