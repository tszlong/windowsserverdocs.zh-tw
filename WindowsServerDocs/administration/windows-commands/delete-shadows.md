---
title: delete shadows
description: 刪除陰影命令的參考文章，此命令會刪除陰影複製。
ms.topic: reference
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f8fe95ac21c36f4605a544c97036c0a02bddaf42
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628855"
---
# <a name="delete-shadows"></a>delete shadows

刪除陰影複製。

## <a name="syntax"></a>語法

```
delete shadows [all | volume <volume> | oldest <volume> | set <setID> | id <shadowID> | exposed {<drive> | <mountpoint>}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---- | ---- |
| all | 刪除所有陰影複製。 |
| 體積 `<volume>` | 刪除指定磁片區的所有陰影複製。 |
| 古老 `<volume>` | 刪除指定磁片區最舊的陰影複製。 |
| 設置 `<setID>` | 刪除指定識別碼之陰影複製集中的陰影複製。 **%** 如果別名存在於目前的環境中，您可以使用符號來指定別名。 |
| Id `<shadowID>` | 刪除指定識別碼的陰影複製。 **%** 如果別名存在於目前的環境中，您可以使用符號來指定別名。 |
| 已公開 {'<drive> | <mountpoint>} |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [delete 命令](delete.md)
