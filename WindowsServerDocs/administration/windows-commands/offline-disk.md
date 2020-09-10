---
title: offline disk
description: 離線磁片命令的參考文章，此命令會將焦點放在離線狀態的線上磁片。
ms.topic: reference
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 678ab63327523e06ae4946413557cc7d45466de0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627401"
---
# <a name="offline-disk"></a>offline disk

將具有焦點的線上磁片帶到離線狀態。 如果磁片群組中的動態磁碟離線，磁片的狀態將會變更為 [ **遺失** ]，而群組會顯示已離線的磁片。 遺失的磁片會移至不正確群組。 如果動態磁碟是群組中的最後一個磁片，則磁片的狀態會變更為 [ **離線**]，並移除空白群組。

> [!NOTE]
> 必須選取磁片， **離線磁片** 命令才能成功。 使用 [ [選取磁片](select-disk.md) ] 命令來選取磁片，並將焦點移至該磁片。
>
> 此命令也可透過將 SAN 模式變更為離線，在 SAN 線上模式中的磁片上運作。

## <a name="syntax"></a>語法

```
offline disk [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

### <a name="examples"></a>範例

若要讓具有焦點的磁片離線，請輸入：

```
offline disk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
