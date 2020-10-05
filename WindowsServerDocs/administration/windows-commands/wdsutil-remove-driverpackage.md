---
title: wdsutil 移除-driverpackage
description: Wdsutil driverpackage 的參考文章，它會從伺服器移除驅動程式套件。
ms.topic: reference
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fe40c9a1dbbf4a5f0bd09e61f39e6b9314a09337
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729852"
---
# <a name="wdsutil-remove-driverpackage"></a>wdsutil 移除-driverpackage

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器移除驅動程式套件。

## <a name="syntax"></a>語法
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>參數

|        參數        |                                                                            描述                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： <Server name> ] |              指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。              |
| [/DriverPackage： <Name> ] |                                                        指定要移除之驅動程式套件的名稱。                                                         |
|    [/PackageId： <ID> ]    | 指定要移除之驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法依名稱唯一識別，則必須指定識別碼。 |

## <a name="examples"></a>範例
若要查看影像的相關資訊，請輸入下列其中一項：
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
- [wdsutil remove-driverpackages 命令](wdsutil-remove-driverpackages.md)
