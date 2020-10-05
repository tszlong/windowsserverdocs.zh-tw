---
title: wdsutil get-driverpackagefile
description: Wdsutil driverpackagefile 的參考文章，它會顯示驅動程式套件的相關資訊，包括驅動程式封裝的相關資訊，包括驅動程式和所包含的檔案。
ms.topic: reference
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f85d57a81aa63f6ecb94b4b09a1614403aa19664
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729904"
---
# <a name="wdsutil-get-driverpackagefile"></a>wdsutil get-driverpackagefile

顯示驅動程式套件的相關資訊，包括驅動程式和其包含的檔案。

## <a name="syntax"></a>語法

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>參數

|         參數         |                              描述                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile:\<Inf File path> | 指定驅動程式套件 .inf 檔案的完整路徑和檔案名。 |
|    [/Architecture： {x86    |                                  ia64                                  |
|     [/Show： {驅動程式      |                                 檔案儲存體                                  |

## <a name="examples"></a>範例

若要查看驅動程式檔案的相關資訊，請輸入：
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)