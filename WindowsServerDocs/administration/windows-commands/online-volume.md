---
title: online volume
description: 線上磁片區命令的參考文章，此命令會將離線磁片區設為線上狀態。
ms.topic: reference
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8847f340ebe3e295946550c46fd86bd59de300af
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032686"
---
# <a name="online-volume"></a>online volume

使離線磁片區處於線上狀態。 此命令適用于失敗、失敗或處於失敗的冗余狀態的磁片區。

> [!NOTE]
> 必須選取磁片區， **線上磁片** 區命令才能成功。 使用 [ [選取磁片](select-volume.md) 區] 命令來選取磁片區，並將焦點移至該磁片區。

> [!IMPORTANT]
> 如果在唯讀磁片上使用此命令，此命令將會失敗。

## <a name="syntax"></a>語法

```
online volume [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="examples"></a>範例

若要讓具有焦點的磁片區上線，請輸入：

```
online volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
