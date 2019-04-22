---
title: mklink
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eabbf159a64ab5df7f45ece390d0c2fdb9956b80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826329"
---
# <a name="mklink"></a>mklink
建立符號連結。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/d|建立目錄的符號連結。 根據預設， **mklink**建立檔案的符號連結。|
|/h|建立永久連結，而不是符號連結。|
|/j|建立目錄連接點。|
|\<連結 >|指定正在建立符號連結的名稱。|
|\<Target>|指定新的符號連結是指的路徑 （相對或絕對）。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>範例

若要建立名為 MyDocs 根目錄 \Users\User1\Documents 目錄的符號連結，請輸入：
```
mklink /d \MyDocs \Users\User1\Documents
```
## <a name="additional-references"></a>其他參考資料
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
