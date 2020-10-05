---
title: wdsutil add-alldriverpackages
description: Wdsutil alldriverpackages 的參考文章，會將儲存在資料夾中的所有驅動程式封裝新增至伺服器。
ms.topic: reference
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d3ec9b3d61c481eacd192ff74e149dbb56134b17
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730041"
---
# <a name="wdsutil-add-alldriverpackages"></a>wdsutil add-alldriverpackages

將儲存在資料夾中的所有驅動程式封裝新增至伺服器。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

### <a name="parameters"></a>參數

|          參數           |                                                              描述                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  FolderPath\<Folder Path>  |                      指定包含驅動程式套件 .inf 檔案之資料夾的完整路徑。                      |
|   [/Server： \<Server name> ]   | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
|     [/Architecture： {x86      |                                                                 ia64                                                                  |
| [/DriverGroup： \<Group Name> ] |                             指定要新增封裝的驅動程式組名。                             |

## <a name="examples"></a>範例

若要新增驅動程式套件，請輸入下列其中一項：
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wsdutil 新增-wdsdriverpackage](/previous-versions/windows/powershell-scripting/dn283440(v=wps.630))
