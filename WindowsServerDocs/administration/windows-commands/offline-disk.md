---
title: offline disk
description: 離線磁片命令的參考文章，這會將線上磁片的焦點放在離線狀態。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc8a746e09e5756eec1890d1715a88e60696cd49
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936755"
---
# <a name="offline-disk"></a>offline disk

將具有焦點的線上磁片帶到離線狀態。 如果磁片群組中的動態磁碟已離線，則磁片的狀態會變更為 [**遺失**]，而群組會顯示離線的磁片。 遺失的磁片會移至不正確群組。 如果動態磁碟是群組中的最後一個磁片，則磁片的狀態會變更為 [**離線**]，而空的群組則會被移除。

> [!NOTE]
> 必須選取磁片，[**離線磁片**] 命令才會成功。 使用 [[選取磁片](select-disk.md)] 命令來選取磁片，並將焦點移至它。
>
> 此命令也適用于 SAN online 模式中的磁片，方法是將 SAN 模式變更為離線。

## <a name="syntax"></a>語法

```
offline disk [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

### <a name="examples"></a>範例

若要讓磁片具有離線焦點，請輸入：

```
offline disk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
