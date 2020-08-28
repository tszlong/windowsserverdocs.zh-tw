---
title: create partition logical
description: Create partition logical 命令的參考文章，會在現有的延伸磁碟分割中建立邏輯資料分割。
ms.topic: reference
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9a95e735fcaed0e7f588a3d4ba643c1787782b5
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033236"
---
# <a name="create-partition-logical"></a>create partition logical

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在現有的延伸磁碟分割上建立邏輯資料分割。 建立磁碟分割之後，焦點會自動移到新磁碟分割。

>[!IMPORTANT]
> 您只能將此命令用於主開機記錄 (MBR) 磁片。 您必須使用 [ [選取磁片](select-disk.md) ] 命令來選取基本的 MBR 磁碟，並將焦點移至該磁片。
>
> 您必須先建立 [延伸磁碟分割](create-partition-extended.md) ，才能建立邏輯磁碟機。

## <a name="syntax"></a>語法

```
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 指定邏輯分割區的大小（以 mb 為單位） (MB) ，其必須小於延伸磁碟分割。 如果未指定大小，則磁碟分割會繼續，直到延伸磁碟分割中的可用空間不足為止。 |
| offset =`<n>` | 以 kb 為單位，指定要在其中建立資料分割的位移（以 kb 為單位） (KB) 。 位移會無條件舍入，以完全填滿使用的任何圓柱大小。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。 磁碟分割的長度至少要與**size = `<n>` **所指定的數位相同。 如果您指定邏輯分割區的大小，它必須小於延伸磁碟分割。 |
| align =`<n>` | 將所有磁片區或磁碟分割範圍對齊到最接近的對齊界限。 通常會與硬體 RAID 邏輯單元編號搭配使用 (LUN) 陣列以改善效能。 `<n>` 這是從磁片開頭到最接近對齊界限的 kb (KB) 數目。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

#### <a name="remarks"></a>備註

- 如果未指定 **大小** 和 **位移** 參數，則會在延伸磁碟分割中可用的最大磁片範圍內建立邏輯分割區。

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的邏輯分割區，請在所選磁片的延伸磁碟分割中輸入：

```
create partition logical size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [create 命令](create.md)

- [select disk](select-disk.md)
