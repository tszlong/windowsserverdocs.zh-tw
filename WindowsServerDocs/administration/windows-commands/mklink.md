---
title: mklink
description: Mklink 命令的參考文章，此命令會建立目錄或檔案符號或硬式連結。
ms.topic: reference
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: dd605283396757f1ef0de05d620583c895f71f1a
ms.sourcegitcommit: f18097c21e50a09aef2f1937f52608b0042ef0e1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96755305"
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

若要從根目錄建立名為 MyFolder 的符號連結，並將其從根目錄中移除，並將名為 Myfile.txt 的硬式連結，輸入到目錄中的範例檔案，請輸入：

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
