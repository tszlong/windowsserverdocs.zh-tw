---
title: unexpose
description: Diskshadow.exe 取消公開命令的參考文章，unexposes 公開的陰影複製。
ms.topic: reference
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 35e545d632790f8c4d7d69d6e9fea1e7e1a07dc4
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156318"
---
# <a name="unexpose"></a>unexpose

Unexposes 使用 [公開命令](expose.md)公開的陰影複製。 公開的陰影複製可透過其陰影識別碼、磁碟機號、共用或掛接點來指定。

## <a name="syntax"></a>語法

```
unexpose {<shadowID> | <drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<shadowID>` | 顯示由指定的陰影識別碼指定的陰影複製。 您可以使用現有的別名或環境變數來取代 `<shadowID>` 。 使用不含參數的 [add 命令](add.md) 以查看所有現有的別名。 |
| `<drive:>` | 顯示與指定的磁碟機號相關聯的陰影複製 (例如，磁片磁碟機 P) 。 |
| `<share>` | 顯示與指定的共用相關聯的陰影複製 (例如 `\\MachineName`) 。 |
| `<mountpoint>` | 顯示與指定掛接點相關聯的陰影複製 (例如 `C:\shadowcopy\`) 。 |
| add | 使用時不含參數，會顯示現有的別名。 |

## <a name="examples"></a>範例

若要 diskshadow.exe 取消公開與 * 磁片磁碟機 P：相關聯的陰影複製 \* ，請輸入：

```
unexpose P:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [新增命令](add.md)

- [公開命令](expose.md)
