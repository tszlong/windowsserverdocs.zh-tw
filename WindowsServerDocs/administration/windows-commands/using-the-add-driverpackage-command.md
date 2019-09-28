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
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f5370d301f5fec15f4812b3d65588297d179455d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363748"
---
# <a name="using-the-add-driverpackage-command"></a>使用 DriverPackage 命令



將驅動程式套件新增至伺服器。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>參數

|          參數           |                                                              描述                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile： \<Inf 檔案路徑 >   |                                           指定要加入之 .inf 檔案的完整路徑。                                            |
|    /Server： \<Server 名稱 >    | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
|      /Architecture： {x86      |                                                                 ia64                                                                  |
| [/DriverGroup： \<Group 名稱 >] |                             指定要新增封裝的驅動程式組名。                              |
|   [/Name： \<Friendly 名稱 >]   |                                           指出驅動程式套件的易記名稱。                                            |

## <a name="BKMK_examples"></a>典型

若要新增驅動程式套件，請輸入下列其中一項：
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

