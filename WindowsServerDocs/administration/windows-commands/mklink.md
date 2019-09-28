---
title: mklink
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9d930cbf7acbfceab16f2fa619aaaac6e789c131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373636"
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
|/d|建立目錄符號連結。 根據預設， **mklink**會建立檔案符號連結。|
|/h|建立硬式連結，而不是符號連結。|
|/j|建立目錄連接點。|
|\<Link >|指定正在建立之符號連結的名稱。|
|\<Target >|指定新符號連結所參考的路徑（相對或絕對）。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>典型

以下範例示範如何建立和移除名為 MyFolder 的符號連結，以及從根目錄到 \Users\User1\Documents 目錄的 Myfile.txt，以及位於目錄內的範例檔案：
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>其他參考資料
-   [新專案](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
