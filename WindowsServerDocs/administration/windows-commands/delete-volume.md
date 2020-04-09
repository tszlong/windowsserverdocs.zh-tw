---
title: delete volume
description: 刪除磁片區的 Windows 命令主題，這會刪除選取的磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2e958785c278306563999b09c1fecc0fdfa7ecb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846551"
---
# <a name="delete-volume"></a>delete volume

刪除選取的磁碟區。

## <a name="syntax"></a>語法

```
delete volume [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="remarks"></a>備註

-   您無法刪除系統磁碟區、開機磁碟區，或任何包含作用中分頁檔案或損毀傾印 (記憶體傾印) 的磁碟區。
-   必須選取一個磁碟區，這項操作才能繼續。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要刪除具有焦點的磁片區，請輸入：
```
delete volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

