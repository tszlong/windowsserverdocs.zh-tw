---
title: create volume mirror
description: 建立磁片區鏡像命令的參考文章，此命令會使用兩個指定的動態磁碟來建立磁片區鏡像。
ms.topic: reference
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8002896bdd84c0fba7dc8eaf2680c100b62ebda9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629098"
---
# <a name="create-volume-mirror"></a>create volume mirror

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用兩個指定的動態磁碟來建立磁片區鏡像。 建立磁片區之後，焦點會自動移至新的磁片區。

## <a name="syntax"></a>語法

```
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 指定磁片區將在每個磁片上佔用的磁碟空間量（以 mb 為單位的 (MB) ）。 如果未指定大小，則新磁碟區會佔用最小磁碟上剩餘的可用空間，及每個後續磁碟上等量的空間。 |
| disk = `<n>` ， `<n>` [ `,<n>,...` ] | 指定建立鏡像磁片區的動態磁碟。 您需要兩個動態磁碟來建立鏡像磁碟區。 會在每個磁片上配置等於以 **size** 參數指定大小的空間量。 |
| align =`<n>` | 將所有磁片區範圍對齊到最接近的對齊界限。 此參數通常會與硬體 RAID 邏輯單元編號搭配使用 (LUN) 陣列，以改善效能。 `<n>` 這是從磁片開頭到最接近對齊界限的 kb (KB) 數目。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並出現錯誤。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的鏡像磁片區，請在磁片1和2上輸入：

```
create volume mirror size=1000 disk=1,2
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [create 命令](create.md)
