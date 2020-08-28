---
title: '復原 (DiskPart) '
description: DiskPart 復原命令的參考文章，會重新整理磁片群組中所有磁片的狀態、嘗試復原無效磁片群組中的磁片，以及重新同步具有過時資料的鏡像磁片區和 RAID-5 磁片區。
ms.topic: reference
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8074fa6ce5edb87911d4c2f774cb0402cd6f41d9
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025092"
---
# <a name="recover-diskpart"></a>復原 (DiskPart) 

重新整理磁片群組中所有磁片的狀態、嘗試復原無效磁片群組中的磁片，以及重新同步具有過時資料的鏡像磁片區和 RAID-5 磁片區。 此命令會在失敗或失敗的磁片上運作。 它也會在失敗、失敗或處於失敗的冗余狀態的磁片區上運作。

此命令會在動態磁碟群組上運作。 如果此命令用於具有基本磁碟的群組，則不會傳回錯誤，但不會採取任何動作。

> [!NOTE]
> 必須選取屬於磁片群組一部分的磁片，此作業才會成功。 使用 [ [選取磁片] 命令](select-disk.md) 來選取磁片，並將焦點移至該磁片。

## <a name="syntax"></a>語法

```
recover [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要復原包含具有焦點磁片的磁片群組，請輸入：

```
recover
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
