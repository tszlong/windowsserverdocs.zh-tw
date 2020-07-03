---
title: delete partition
description: 刪除資料分割命令的參考文章，這會刪除具有焦點的資料分割。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b45cb060a5d82e254fe371269dbdbcb9d46fee92
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928726"
---
# <a name="delete-partition"></a>delete partition

刪除具有焦點的資料分割。 開始之前，您必須選取資料分割，此作業才會成功。 使用 [[選取資料分割](select-partition.md)] 命令來選取磁碟分割，並將焦點移至該資料分割。

> [!WARNING]
> 刪除動態磁碟上的磁碟分割可以刪除磁片上的所有動態磁碟區，終結任何資料並讓磁片處於損毀狀態。
>
> 您無法刪除系統磁碟分割、開機磁碟分割，或包含作用中分頁檔案或損毀傾印資訊的任何磁碟分割。

## <a name="syntax"></a>語法

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |
| override | 啟用 DiskPart 來刪除各種類型的磁碟分割。 一般來說，DiskPart 只允許您刪除已知的資料磁碟分割。 |

#### <a name="remarks"></a>備註

- 若要刪除動態磁碟區，請一律改用 [[刪除磁片](delete-volume.md)區] 命令。

- 磁碟分割可以從動態磁碟中刪除，但不應建立。 例如，您可以刪除動態 GPT 磁片上無法辨識的 GUID 磁碟分割表格（GPT）分割區。 刪除這類資料分割並不會導致產生的可用空間變成可用。 相反地，此命令的目的是讓您在無法使用 DiskPart 中的[clean](clean.md)命令的緊急情況下，回收損毀離線動態磁碟上的空間。

## <a name="examples"></a>範例

若要刪除具有焦點的資料分割，請輸入：

```
delete partition
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [select partition](select-partition.md)

- [delete 命令](delete.md)

- [刪除磁片區命令](delete-volume.md)

- [clean 命令](clean.md)
