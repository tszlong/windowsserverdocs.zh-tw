---
title: mklink
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83a607711f0abe51810aa5abf4eb731206d810c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723978"
---
# <a name="mklink"></a>mklink
建立符號連結。



## <a name="syntax"></a>語法

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/d|建立目錄符號連結。 根據預設， **mklink**會建立檔案符號連結。|
|/h|建立硬式連結，而不是符號連結。|
|/j|建立目錄連接點。|
|\<連結>|指定正在建立之符號連結的名稱。|
|\<Target>|指定新符號連結所參考的路徑（相對或絕對）。|
|/?|在命令提示字元顯示說明。|

## <a name="examples"></a>範例

示範如何建立和移除名為 MyFolder 和 Myfile.txt 的符號連結，從根目錄到 \Users\User1\Documents 目錄，以及位於目錄中的範例檔案：
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
