---
title: wdsutil get-driverpackagefile
description: Wdsutil driverpackagefile 的參考文章，它會顯示驅動程式套件的相關資訊，包括驅動程式封裝的相關資訊，包括驅動程式和所包含的檔案。
ms.topic: reference
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 900fc109c52908870733d4892e6d70f4a7b84c07
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524263"
---
# <a name="wdsutil-get-driverpackagefile"></a>wdsutil get-driverpackagefile

顯示驅動程式套件的相關資訊，包括驅動程式和其包含的檔案。

## <a name="syntax"></a>語法

```
wdsutil /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>參數

|         參數         |                              說明                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile:\<Inf File path> | 指定驅動程式套件 .inf 檔案的完整路徑和檔案名。 |
|    [/Architecture： {x86    |                                  ia64                                  |
|     [/Show： {驅動程式      |                                 檔案                                  |

## <a name="examples"></a>範例

若要查看驅動程式檔案的相關資訊，請輸入：
```
wdsutil /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)