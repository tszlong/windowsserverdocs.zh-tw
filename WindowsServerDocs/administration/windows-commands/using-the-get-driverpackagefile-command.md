---
title: DriverPackageFile
description: DriverPackageFile 的參考主題，它會顯示驅動程式套件的相關資訊，包括其內含的驅動程式和檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 71fc38e31471a1deb9d6be29b04d3cd911be1bd6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719927"
---
# <a name="get-driverpackagefile"></a>DriverPackageFile

顯示驅動程式套件的相關資訊，包括其內含的驅動程式和檔案。

## <a name="syntax"></a>語法

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>參數

|         參數         |                              描述                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile：\<Inf 檔案路徑> | 指定驅動程式套件 .inf 檔案的完整路徑和檔案名。 |
|    [/Architecture： {x86    |                                  ia64                                  |
|     [/Show： {驅動程式      |                                 檔案                                  |

## <a name="examples"></a>範例

若要查看驅動程式檔案的相關資訊，請輸入：
```
WDSUTIL /Get-DriverPackageFile /InfFile:C:\temp\1394.inf /Architecture:x86
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)