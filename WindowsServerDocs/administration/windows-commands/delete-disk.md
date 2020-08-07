---
title: delete disk
description: 刪除磁片命令的參考文章，它會從磁片清單刪除遺失的動態磁碟。
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85657e571fb5f0f4e0a55cf54065056a8f28c385
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891447"
---
# <a name="delete-disk"></a>delete disk

從磁片清單刪除遺失的動態磁碟。

> [!NOTE]
> 如需如何使用此命令的詳細指示，請參閱[移除遺失的動態磁碟](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc753029(v=ws.11))。

## <a name="syntax"></a>語法

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |
| override | 啟用 DiskPart 來刪除磁碟上的全部簡單磁碟區。 如果磁碟包含鏡像磁碟區的一半，則會刪除磁碟上的一半鏡像。 如果磁碟屬於 RAID-5 磁碟區，則 delete disk override 命令失敗。 |

## <a name="examples"></a>範例

若要從磁片清單中刪除遺失的動態磁碟，請輸入：

```
delete disk
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [delete 命令](delete.md)
