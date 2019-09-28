---
title: 使用 DriverPackage 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3d31d9a02454b0f7fca06b28a4df27174f7b02e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363186"
---
# <a name="using-the-get-driverpackage-command"></a>使用 DriverPackage 命令



顯示伺服器上驅動程式套件的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>參數

|        參數         |                                                                           描述                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： \<Server 名稱 >] |              指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。               |
| [/DriverPackage： \<Name >] |                                                        指定要顯示的驅動程式套件的名稱。                                                         |
|    [/PackageId： \<ID >]    | 指定要顯示之驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法以名稱唯一識別，您就必須指定識別碼。 |
|     [/Show： {驅動程式     |                                                                              檔案                                                                               |

## <a name="BKMK_examples"></a>典型

若要查看驅動程式套件的相關資訊，請輸入下列其中一項：
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)