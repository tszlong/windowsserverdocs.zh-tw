---
title: DriverPackage
description: DriverPackage 的 Windows 命令主題，它會顯示伺服器上驅動程式套件的相關資訊。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1906a109d22b24b5a44227d56c726996e6532bd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831051"
---
# <a name="get-driverpackage"></a>DriverPackage

顯示伺服器上驅動程式套件的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>參數

|        參數         |                                                                           描述                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server：\<伺服器名稱 >] |              指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。               |
| [/DriverPackage：\<名稱 >] |                                                        指定要顯示的驅動程式套件的名稱。                                                         |
|    [/PackageId：\<識別碼 >]    | 指定要顯示之驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法以名稱唯一識別，您就必須指定識別碼。 |
|     [/Show： {驅動程式     |                                                                              Files                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要查看驅動程式套件的相關資訊，請輸入下列其中一項：
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)