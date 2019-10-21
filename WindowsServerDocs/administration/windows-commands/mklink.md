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
ms.openlocfilehash: b7a5f9b819f16d058feb1dee74a8408ed174e04c
ms.sourcegitcommit: b7f55949f166554614f581c9ddcef5a82fa00625
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72588046"
---
# <a name="mklink"></a>mklink
建立符號連結。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

## <a name="parameters"></a>Parameters

|參數|說明|
|---------|-----------|
|/d|建立目錄符號連結。 根據預設， **mklink**會建立檔案符號連結。|
|/h|建立硬式連結，而不是符號連結。|
|/j|建立目錄連接點。|
|\<Link >|指定正在建立之符號連結的名稱。|
|\<Target >|指定新符號連結所參考的路徑（相對或絕對）。|
|/?|在命令提示字元顯示說明。|

## <a name="BKMK_examples"></a>典型

下列範例示範如何建立和移除名為 MyFolder 和 Myfile.txt 的符號連結，從根目錄到 \Users\User1\Documents 目錄，以及位於目錄中的範例檔案：
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
