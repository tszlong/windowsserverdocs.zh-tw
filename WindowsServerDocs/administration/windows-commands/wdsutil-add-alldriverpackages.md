---
title: wdsutil add-alldriverpackages
description: Wdsutil add alldriverpackages 命令的參考文章，此命令會將儲存在資料夾中的所有驅動程式封裝新增至伺服器。
ms.topic: reference
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 07d958fb382f040db34f486a28fed4b924ce45d8
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524653"
---
# <a name="wdsutil-add-alldriverpackages"></a>wdsutil add-alldriverpackages

將儲存在資料夾中的所有驅動程式封裝新增至伺服器。

## <a name="syntax"></a>語法

```
wdsutil /Add-AllDriverPackages /FolderPath:<folderpath> [/Server:<servername>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<groupname>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| FolderPath`<folderpath>` | 指定包含驅動程式套件 .inf 檔案之資料夾的完整路徑。 |
| [/Server： `<servername>` ] | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
| [/Architecture： `{x86|ia64|x64}` ] | 指定驅動程式套件的架構類型。 |
| [/DriverGroup： `<groupname>` ] | 指定要新增封裝的驅動程式組名。 |

## <a name="examples"></a>範例

若要新增驅動程式套件，請輸入下列其中一項：

```
wdsutil /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```

```
wdsutil /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 部署服務 Cmdlet](/powershell/module/wds)

- [新增-WdsDriverPackage](/powershell/module/wds/add-wdsdriverpackage)
