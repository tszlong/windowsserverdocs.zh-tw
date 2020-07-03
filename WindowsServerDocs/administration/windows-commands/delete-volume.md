---
title: delete volume
description: 刪除磁片區命令的參考文章，這會刪除選取的磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 217e9ff1ccb470b5431143360286d312b09a1951
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928711"
---
# <a name="delete-volume"></a>delete volume

刪除選取的磁碟區。 開始之前，您必須選取磁片區，此作業才會成功。 使用 [[選取磁片](select-volume.md)區] 命令來選取磁片區，並將焦點移至該磁片區。

> [!IMPORTANT]
> 您無法刪除系統磁碟區、開機磁碟區，或包含作用中分頁檔案或損毀傾印（記憶體傾印）的任何磁片區。

## <a name="syntax"></a>語法

```
delete volume [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要刪除具有焦點的磁片區，請輸入：

```
delete volume
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [select volume](select-volume.md)

- [delete 命令](delete.md)