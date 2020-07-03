---
title: 修復
description: '* * * * 的參考文章'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e9afb6c418558078496871b71bfaff706753b0a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936759"
---
# <a name="repair"></a>修復

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以指定的 \- 動態磁碟取代失敗的磁片區域，以修復具有焦點的 RAID 5 磁片區。



## <a name="syntax"></a>語法

```
repair disk=<n> [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數  |                                                                                             說明                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 硬碟\=<n>  |                                                                 指定將取代失敗磁片區域的動態磁碟。                                                                 |
| 對齊\=<n> |          將所有磁片區或資料分割範圍對齊最接近的對齊界限。 *n*是 \( \) 從磁片開頭到最接近對齊界限的 kb kb 數。           |
|   noerr    | 僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="remarks"></a>備註

-   指定的動態磁碟的可用空間必須大於或等於 RAID 5 磁片區中失敗磁片區域的總大小 \- 。

-   必須選取 RAID 5 陣列中的磁片區 \- ，此操作才能成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。

## <a name="examples"></a>範例
若要將具有焦點的磁片區取代為動態磁碟4，請輸入：

```
repair disk=4
```

## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)




