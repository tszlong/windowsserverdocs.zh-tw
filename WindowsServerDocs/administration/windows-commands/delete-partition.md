---
title: delete partition
description: 刪除資料分割命令的參考文章，此命令會刪除具有焦點的資料分割。
ms.topic: reference
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2833039c9237a271910c43ff8acb7b8fb94828f1
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628835"
---
# <a name="delete-partition"></a>delete partition

刪除具有焦點的資料分割。 開始之前，您必須選取分割區，此作業才會成功。 使用 [ [選取資料分割](select-partition.md) ] 命令來選取分割區，並將焦點移至該分割區。

> [!WARNING]
> 刪除動態磁碟上的磁碟分割可以刪除磁片上的所有動態磁碟區，終結任何資料並使磁片處於損毀狀態。
>
> 您無法刪除系統磁碟分割、開機磁碟分割或任何包含作用中分頁檔案或損毀傾印資訊的磁碟分割。

## <a name="syntax"></a>語法

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |
| override | 啟用 DiskPart 來刪除各種類型的磁碟分割。 一般來說，DiskPart 只允許您刪除已知的資料磁碟分割。 |

#### <a name="remarks"></a>備註

- 若要刪除動態磁碟區，請一律改用 [刪除磁片](delete-volume.md) 區命令。

- 您可以從動態磁碟刪除磁碟分割，但是不應該建立。 例如，您可以在動態 GPT 磁片上刪除無法辨識的 GUID 磁碟分割表 (GPT) 磁碟分割。 刪除這類分割區並不會導致產生的可用空間變成可用空間。 相反地，此命令的目的是要讓您在不能使用 DiskPart 中的 [clean](clean.md) 命令的緊急情況下，回收損毀離線動態磁碟上的空間。

## <a name="examples"></a>範例

若要刪除具有焦點的磁碟分割，請輸入：

```
delete partition
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [select partition](select-partition.md)

- [delete 命令](delete.md)

- [刪除磁片區命令](delete-volume.md)

- [clean 命令](clean.md)
