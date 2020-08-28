---
title: offline volume
description: 離線磁片區命令的參考文章，此命令會將焦點放在離線狀態的線上磁片區。
ms.topic: reference
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c38a38e8e907cc557f0c13690de3941c10f4065
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032736"
---
# <a name="offline-volume"></a>offline volume

將具有焦點的線上磁片區帶到離線狀態。

> [!NOTE]
> 必須選取磁片區， **離線磁片** 區命令才能成功。 使用 [ [選取](select-volume.md) 磁片區] 命令來選取磁片，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
offline volume [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="examples"></a>範例

若要讓具有焦點的磁片離線，請輸入：

```
offline volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
