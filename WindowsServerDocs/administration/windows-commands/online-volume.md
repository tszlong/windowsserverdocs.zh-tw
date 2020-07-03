---
title: online volume
description: 線上磁片區命令的參考文章，它會將離線磁片區帶到線上狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2378b4cbc4f0624a0f1d65c62337d4c1a856648c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933525"
---
# <a name="online-disk"></a>online disk

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

| 參數 | 說明 |
|--|--|
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="examples"></a>範例

若要讓磁片區與焦點上線，請輸入：

```
online volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
