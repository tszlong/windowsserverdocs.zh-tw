---
title: 移除-DriverGroupPackage
description: DriverGroupPackage 的參考文章，它會從伺服器上的驅動程式群組移除驅動程式套件。
ms.topic: article
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a677204f593c5c9b791d0e68f726cc75c6f94764
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892397"
---
# <a name="remove-drivergrouppackage"></a>移除-DriverGroupPackage



將驅動程式套件從伺服器上的驅動程式群組移除。

## <a name="syntax"></a>語法

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[/Server： \<Server name> ]|指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。|
|[/DriverPackage： \<Name> ]|指定要移除之驅動程式套件的名稱。|
|[/PackageId： \<ID> ]|指定要移除之驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法以名稱唯一識別，您就必須指定此選項。|

## <a name="examples"></a>範例

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)