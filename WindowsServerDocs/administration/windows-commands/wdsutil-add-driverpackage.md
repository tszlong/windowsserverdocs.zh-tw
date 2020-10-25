---
title: 新增-DriverPackage
description: DriverPackage 命令的參考文章，可將驅動程式套件新增至伺服器。
ms.topic: reference
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c85279cc160ca12b52f4cf7225d0ac41700973bc
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524520"
---
# <a name="add-driverpackage"></a>新增-DriverPackage

將驅動程式套件新增至伺服器。

## <a name="syntax"></a>語法

```
wdsutil /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /InfFile:`<InfFilepath>` | 指定要新增之 .inf 檔案的完整路徑。 |
| [/Server： `<Servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| [/Architecture： `{x86 | ia64 | x64}` ] | 指定驅動程式套件的架構類型。 |
| [/DriverGroup： `<groupname>` ] | 指定要新增封裝的驅動程式組名。 |
| [/Name： `<friendlyname>` ] | 指定驅動程式套件的易記名稱。 |

## <a name="examples"></a>範例

若要新增驅動程式套件，請輸入下列其中一項：

```
wdsutil /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```

```
wdsutil /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wdsutil add-drivergrouppackage 命令](wdsutil-add-drivergrouppackage.md)

- [wdsutil add-alldriverpackages 命令](wdsutil-add-alldriverpackages.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)
