---
title: online volume
description: 線上磁片區命令的參考文章，它會將離線磁片區帶到線上狀態。
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b09730d3cc0cfe758c90c3ca57fd039282ba3fce
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885222"
---
# <a name="online-volume"></a>online volume

使離線卷回到線上狀態。 此命令適用于故障、失敗或處於失敗的冗余狀態的磁片區。

> [!NOTE]
> 必須選取磁片區，**線上磁片**區命令才會成功。 使用 [[選取磁片](select-volume.md)區] 命令來選取磁片區，並將焦點移至該磁片區。

> [!IMPORTANT]
> 如果用於唯讀磁片，此命令將會失敗。

## <a name="syntax"></a>語法

```
online volume [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="examples"></a>範例

若要讓磁片區與焦點上線，請輸入：

```
online volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
