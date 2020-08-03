---
title: 復原（DiskPart）
description: DiskPart 復原命令的參考文章，它會重新整理磁片群組中所有磁片的狀態、嘗試復原無效磁片群組中的磁片，以及重新同步擁有過時資料的鏡像磁碟區和 RAID-5 磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03b996db5d8c6d270a35bf1273570e3757258658
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519677"
---
# <a name="recover-diskpart"></a>復原（DiskPart）

重新整理磁片群組中所有磁片的狀態、嘗試復原無效磁片群組中的磁片，以及重新同步擁有過時資料的鏡像磁碟區和 RAID-5 磁片區。 此命令會在失敗或失敗的磁片上操作。 它也會在失敗、失敗或處於失敗的冗余狀態的磁片區上運作。

此命令會在動態磁碟群組上運作。 如果在具有基本磁碟的群組上使用此命令，它不會傳回錯誤，但不會採取任何動作。

> [!NOTE]
> 必須選取屬於磁片群組一部分的磁片，此操作才能成功。 使用 [[選取磁片] 命令](select-disk.md)來選取磁片，並將焦點移至它。

## <a name="syntax"></a>語法

```
recover [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要復原包含具有焦點之磁片的磁片群組，請輸入：

```
recover
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
