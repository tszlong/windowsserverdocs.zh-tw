---
title: create partition logical
description: 建立資料分割邏輯命令的參考文章，它會在現有的擴展資料分割中建立邏輯分割區。
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26adcb50c43f859d312dadc16328bc65aed29cee
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879905"
---
# <a name="create-partition-logical"></a>create partition logical

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在現有的延伸磁碟分割上建立邏輯分割區。 建立磁碟分割之後，焦點會自動移到新磁碟分割。

>[!IMPORTANT]
> 您只能在 (MBR) 磁片的主開機記錄上使用此命令。 您必須使用 [[選取磁片](select-disk.md)] 命令來選取基本的 MBR 磁碟，並將焦點移至其上。
>
> 您必須先建立[延伸磁碟分割](create-partition-extended.md)，才能建立邏輯磁碟機。

## <a name="syntax"></a>語法

```
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 指定邏輯分割區的大小（以 mb 為單位） (MB) ，其必須小於延伸磁碟分割。 如果沒有指定大小，磁碟分割會繼續進行，直到延伸磁碟分割中沒有其他可用空間為止。 |
| offset =`<n>` | 指定建立分割區的位移（以 kb 為單位） (KB) 。 位移會向上舍入，以完全填滿使用的任何圓柱大小。 如果沒有指定位移的話，則磁碟分割會置於足夠容納它的第一個磁碟範圍內。 資料分割的長度至少為**size = `<n>` **所指定的數位。 如果您指定了邏輯分割區的大小，它必須小於延伸磁碟分割。 |
| align =`<n>` | 將所有磁片區或資料分割範圍對齊最接近的對齊界限。 通常用於硬體 RAID 邏輯單元編號 (LUN) 陣列以改善效能。 `<n>`這是從磁片開始到最接近的對齊界限的 kb (KB) 數。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

#### <a name="remarks"></a>備註

- 如果未指定**size**和**offset**參數，則會在延伸磁碟分割可用的最大磁片範圍內建立邏輯分割區。

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的邏輯分割區，請在所選磁片的延伸磁碟分割中，輸入：

```
create partition logical size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [建立命令](create.md)

- [select disk](select-disk.md)
