---
title: 線上磁片區
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 476dd893e7899a2bd58336546a7881934f415f92
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837841"
---
# <a name="online-volume"></a>線上磁片區



將目前離線的磁片區帶入線上狀態

> [!IMPORTANT]
> Windows Vista 的任何版本都無法使用此命令。

> [!IMPORTANT]
> 如果在唯讀磁片區上使用此命令，將會失敗。

## <a name="syntax"></a>語法

```
online volume [noerr]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|noerr|僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

-   此命令會在故障、失敗或處於失敗的冗余狀態的磁片區上運作。
-   必須選取磁片區，此命令才會成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要讓磁片區與焦點上線，請輸入：
```
online volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

