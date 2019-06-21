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
ms.openlocfilehash: 1ea80b81b268b31f637c72a828fee8b6f0229a47
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280028"
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

下列範例示範如何建立和移除名為 MyFolder 和 MyFile.file 根目錄 \Users\User1\Documents 目錄的符號連結和 example.file，位於 目錄：
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>其他參考資料
-   [New-Item](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
