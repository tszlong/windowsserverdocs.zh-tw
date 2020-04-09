---
title: 新增-DriverPackage
description: 適用于 DriverPackage 的 Windows 命令主題，它會將驅動程式套件新增至伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2ccdfcddd2f605eccd9cd32fed7b8c6921297fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832011"
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
|   InfFile：\<Inf 檔案路徑 >   |                                           指定要加入之 .inf 檔案的完整路徑。                                            |
|    /Server：\<伺服器名稱 >    | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果未指定伺服器名稱，則會使用本機伺服器。 |
|      /Architecture： {x86      |                                                                 ia64                                                                  |
| [/DriverGroup：\<組名 >] |                             指定要新增封裝的驅動程式組名。                              |
|   [/Name：\<易記名稱 >]   |                                           指出驅動程式套件的易記名稱。                                            |

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要新增驅動程式套件，請輸入下列其中一項：
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

