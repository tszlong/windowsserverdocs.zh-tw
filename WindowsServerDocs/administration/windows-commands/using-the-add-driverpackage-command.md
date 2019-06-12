---
title: 使用新增 DriverPackage 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 893bcd3b28aaa5d501017fe65b7b5205e9452693
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440697"
---
# <a name="using-the-add-driverpackage-command"></a>使用新增 DriverPackage 命令



將驅動程式套件新增到伺服器。

## <a name="syntax"></a>語法

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>參數

|          參數           |                                                              描述                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile:\<Inf 檔案路徑 >   |                                           指定要加入的.inf 檔案的完整路徑。                                            |
|    /Server:\<伺服器名稱 >    | 指定伺服器的名稱。 這可以是 NetBIOS 名稱或 FQDN。 如果沒有指定伺服器名稱時，會使用本機伺服器。 |
|      / 架構: {x86      |                                                                 ia64                                                                  |
| [/ DriverGroup:\<群組名稱 >] |                             指定應加入封裝的驅動程式群組的名稱。                              |
|   [/Name:\<易記名稱 >]   |                                           狀態為驅動程式套件的易記名稱。                                            |

## <a name="BKMK_examples"></a>範例

若要新增驅動程式套件，請輸入下列其中一項：
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

