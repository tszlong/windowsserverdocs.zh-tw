---
title: delete partition
description: 刪除磁碟分割的 Windows 命令主題，這會刪除具有焦點的資料分割。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a24c18cf98f2899fbb57f1f5f2d2776824d637b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846571"
---
# <a name="delete-partition"></a>delete partition

刪除具有焦點的資料分割。

## <a name="syntax"></a>語法

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|覆寫|啟用 DiskPart 來刪除各種類型的磁碟分割。 一般來說，DiskPart 只允許您刪除已知的資料磁碟分割。|
|noerr|僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。|

## <a name="remarks"></a>備註

> [!CAUTION]
> 刪除動態磁碟上的磁碟分割，可刪除磁片上的所有動態磁碟區，因而摧毀所有資料並讓磁片處於損毀狀態。 若要刪除動態磁碟區，請一律改用 [**刪除磁片**區] 命令。 磁碟分割可以從動態磁碟中刪除，但不能建立。 例如，您可在動態 GPT 磁碟上刪除一個無法辨識的 GUID 磁碟分割表格 (GPT) 磁碟分割。 刪除這類分割區並不會導致產生的可用空間變成可用。 此命令的目的是讓您在無法使用 DiskPart 中的**clean**命令的緊急情況下，reclame 損毀離線動態磁碟上的空間。
> -   您無法刪除系統磁碟分割、開機磁碟分割，或包含作用中分頁檔案或損毀傾印資訊的任何磁碟分割。
> -   必須選取一個磁碟分割，這項操作才能繼續。 使用 [**選取資料分割**] 命令來選取磁碟分割，並將焦點移至該資料分割。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要刪除具有焦點的資料分割，請輸入：
```
delete partition
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

