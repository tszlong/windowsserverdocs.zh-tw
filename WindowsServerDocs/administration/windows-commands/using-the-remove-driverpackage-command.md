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
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 923a86805134c4162b36cdade98c2122b3cb7ccd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362814"
---
# <a name="using-the-remove-driverpackage-command"></a>使用 DriverPackage 命令

> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012
> 
> 
> 適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

從伺服器移除驅動程式套件。
## <a name="syntax"></a>語法
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parameters

|        參數        |                                                                            描述                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server：<Server name>] |              指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。              |
| [/DriverPackage：<Name>] |                                                        指定要移除之驅動程式套件的名稱。                                                         |
|    [/PackageId：<ID>]    | 指定要移除之驅動程式套件的 Windows 部署服務識別碼。 如果驅動程式套件無法以名稱唯一識別，您就必須指定識別碼。 |

## <a name="BKMK_examples"></a>典型
若要查看影像的相關資訊，請輸入下列其中一項：
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>其他參考
[使用 DriverPackages 命令](using-the-remove-driverpackages-command.md)
[命令列語法金鑰](command-line-syntax-key.md)
