---
title: create volume raid
description: 建立磁片區 raid 命令的參考文章，它會使用三個或多個指定的動態磁碟建立 RAID-5 磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c3b142e7fd678af04d1bc4e109ac399807da06e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929544"
---
# <a name="create-volume-raid"></a>create volume raid

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用三個或多個指定的動態磁碟，建立 RAID-5 磁片區。 建立磁碟區之後，焦點會自動移到新磁碟區。

## <a name="syntax"></a>語法

```
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 大小 =`<n>` | 磁碟區佔據每個磁碟上的磁碟空間 (以 MB 為單位)。 如果沒有指定大小，將會建立最大可能的 RAID-5 磁片區。 具有最小可用連續剩餘空間的磁碟決定 RAID-5 磁碟區大小，並會從每個磁碟配置相同數量的空間。 因為同位檢查需要部分磁碟空間，所以 RAID-5 磁碟區中可用磁碟空間的實際數量小於磁碟空間的總數量。 |
| 磁片 =`<n>,<n>,<n>[,<n>,...]` | 要在其中建立 RAID-5 磁片區的動態磁碟。 您至少需要三個動態磁碟以建立一個 RAID-5 磁碟區。 每個磁片上配置了等於的空間量 `size=<n>` 。 |
| align =`<n>` | 將所有磁片區範圍對齊最接近的對齊界限。 通常與硬體 RAID 邏輯單元編號（LUN）陣列搭配使用，以改善效能。 `<n>`這是從磁片開始到最接近對齊界限的 kb 數（KB）。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要使用磁片1、2和3來建立大小為 1000 mb 的 RAID-5 磁片區，請輸入：

```
create volume raid size=1000 disk=1,2,3
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [建立命令](create.md)
