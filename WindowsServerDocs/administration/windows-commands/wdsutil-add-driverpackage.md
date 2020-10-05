---
title: 新增-DriverPackage
description: DriverPackage 的參考文章，可將驅動程式套件加入至伺服器。
ms.topic: reference
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b7e950b15aaea152f043f7e9252d05773f05f2f2
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91730006"
---
# <a name="add-driverpackage"></a>新增-DriverPackage

將驅動程式套件新增至伺服器。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>參數

|          參數           |                                                              描述                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile:\<Inf File path>   |                                           指定要新增之 .inf 檔案的完整路徑。                                            |
|    伺服器\<Server name>    | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
|      /Architecture： {x86      |                                                                 ia64                                                                  |
| [/DriverGroup： \<Group Name> ] |                             指定要新增封裝的驅動程式組名。                              |
|   [/Name： \<Friendly Name> ]   |                                           指出驅動程式套件的易記名稱。                                            |

## <a name="examples"></a>範例

若要新增驅動程式套件，請輸入下列其中一項：
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

