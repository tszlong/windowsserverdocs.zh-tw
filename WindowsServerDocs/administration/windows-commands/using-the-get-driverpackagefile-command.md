---
title: 使用 DriverPackageFile 命令
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f01a2c67-7e9c-4aad-b625-383f5a1fca25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21bbe17e56177da5cd2c1bf83c712d256cc794c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363155"
---
# <a name="using-the-get-driverpackagefile-command"></a>使用 DriverPackageFile 命令



顯示驅動程式套件的相關資訊，包括其內含的驅動程式和檔案。

## <a name="syntax"></a>語法

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>參數

|         參數         |                              描述                               |
|---------------------------|------------------------------------------------------------------------|
| /InfFile： \<Inf 檔案路徑 > | 指定驅動程式套件 .inf 檔案的完整路徑和檔案名。 |
|    [/Architecture： {x86    |                                  ia64                                  |
|     [/Show： {驅動程式      |                                 檔案                                  |

## <a name="BKMK_examples"></a>典型

若要查看驅動程式檔案的相關資訊，請輸入：
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)