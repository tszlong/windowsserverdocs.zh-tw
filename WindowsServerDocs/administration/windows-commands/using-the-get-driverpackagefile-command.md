---
title: 使用 get DriverPackageFile 命令
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 264bdb6d51622e6323be00b44014b86cd9662e61
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440498"
---
# <a name="using-the-get-driverpackagefile-command"></a>使用 get DriverPackageFile 命令



顯示驅動程式套件，包括驅動程式和其所包含的檔案的相關資訊。

## <a name="syntax"></a>語法

```
WDSUTIL /Get-DriverPackageFile /InfFile:<Inf File path> [/Architecture:{x86 | ia64 | x64}] [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>參數

|         參數         |                              描述                               |
|---------------------------|------------------------------------------------------------------------|
| / InfFile:\<Inf 檔案路徑 > | 指定驅動程式套件.inf 檔案的完整路徑和檔案名稱。 |
|    [/ 架構: {x86    |                                  ia64                                  |
|     [] / [顯示: {驅動程式      |                                 檔案                                  |

## <a name="BKMK_examples"></a>範例

若要檢視驅動程式檔案的相關資訊，請輸入：
```
WDSUTIL /Get-DriverPackageFile /InfFile:"C:\temp\1394.inf" /Architecture:x86
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)