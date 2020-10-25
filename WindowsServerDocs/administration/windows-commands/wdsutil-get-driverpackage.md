---
title: DriverPackage
description: DriverPackage 的參考文章，會顯示伺服器上驅動程式套件的相關資訊。
ms.topic: reference
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 86f8b2cdc7a0bc62f42c02c9ff7c6852e9b159f0
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524433"
---
# <a name="get-driverpackage"></a>DriverPackage

顯示伺服器上驅動程式套件的相關資訊。

## <a name="syntax"></a>語法

```
wdsutil /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>參數

|        參數         |                                                                           說明                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： \<Server name> ] |              指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。               |
| [/DriverPackage： \<Name> ] |                                                        指定要顯示之驅動程式套件的名稱。                                                         |
|    [/PackageId： \<ID> ]    | 指定要顯示之驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法依名稱唯一識別，則必須指定識別碼。 |
|     [/Show： {驅動程式     |                                                                              檔案                                                                               |

## <a name="examples"></a>範例

若要查看驅動程式套件的相關資訊，請輸入下列其中一項：
```
wdsutil /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
wdsutil /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)