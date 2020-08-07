---
title: delete shadows
description: 刪除陰影命令的參考文章，這會刪除陰影複製。
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29d1679b2d05265aa1fb5a089fab9cf99f840cd9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891373"
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
| 主卷`<volume>` | 刪除指定磁片區的所有陰影複製。 |
| 早`<volume>` | 刪除指定磁片區最舊的陰影複製。 |
| 設定`<setID>` | 刪除指定識別碼之陰影複製組中的陰影複製。 **%** 如果別名存在於目前的環境中，您可以使用符號來指定別名。 |
| 號`<shadowID>` | 刪除指定識別碼的陰影複本。 **%** 如果別名存在於目前的環境中，您可以使用符號來指定別名。 |
| 已公開 {'<drive> | <mountpoint>} |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [delete 命令](delete.md)
