---
title: offline volume
description: 離線磁片區命令的參考文章，它會將線上磁片區的焦點放在離線狀態。
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e8a6bbebb88f97c4e4b04afccedeb05c4a1392a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885262"
---
# <a name="offline-volume"></a>offline volume

將線上磁片區的焦點放在離線狀態。

> [!NOTE]
> 必須選取磁片區，[**離線磁片**區] 命令才會成功。 使用 [[選取](select-volume.md)磁片區] 命令來選取磁片，並將焦點移至它。

## <a name="syntax"></a>語法

```
offline volume [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="examples"></a>範例

若要讓磁片具有離線焦點，請輸入：

```
offline volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
