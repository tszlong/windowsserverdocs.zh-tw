---
title: mklink
description: Mklink 命令的參考文章，此命令會建立目錄或檔案符號或硬式連結。
ms.topic: reference
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7d5c7b971d2ca77308c24210ee50c17c4b3a0c95
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640341"
---
# <a name="mklink"></a>mklink

建立目錄或檔案符號或硬式連結。

## <a name="syntax"></a>語法

```
mklink [[/d] | [/h] | [/j]] <link> <target>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /d | 建立目錄符號連結。 根據預設，此命令會建立檔案符號連結。 |
| /h | 建立硬式連結，而不是符號連結。 |
| /j | 建立目錄連接點。 |
| `<link>` | 指定要建立之符號連結的名稱。 |
| `<target>` | 指定新符號連結所參考 (相對或絕對) 路徑。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="examples"></a>範例

若要建立和移除名為、MyFolder 和 Myfile.txt 的符號連結，請從根目錄到 \Users\User1\Documents 目錄，並將檔案放在目錄中，輸入：

```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [del 命令](del.md)

- [rd 命令](rd.md)

- [Windows PowerShell 中的新專案](/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
