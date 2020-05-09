---
title: 建立磁片區鏡像
description: 建立磁片區鏡像命令的參考主題，它會使用兩個指定的動態磁碟來建立磁片區鏡像。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be6e4496876636351b6e0853626a9ff9bb421f18
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993251"
---
# <a name="create-volume-mirror"></a>建立磁片區鏡像

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用兩個指定的動態磁碟來建立磁片區鏡像。 建立磁片區之後，焦點會自動移至新的磁片區。

## <a name="syntax"></a>語法

```
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 指定磁片區將在每個磁片上佔用的磁碟空間量（以 mb 為單位）。 如果未指定大小，則新磁碟區會佔用最小磁碟上剩餘的可用空間，及每個後續磁碟上等量的空間。 |
| disk =`<n>`，`<n>`[`,<n>,...`] | 指定要在其上建立鏡像磁碟區的動態磁碟。 您需要兩個動態磁碟來建立鏡像磁碟區。 系統會在每個磁片上配置與**size**參數所指定大小相等的空間量。 |
| align =`<n>` | 將所有磁片區範圍對齊最接近的對齊界限。 此參數通常會與硬體 RAID 邏輯單元編號（LUN）陣列搭配使用，以改善效能。 `<n>`這是從磁片開始到最接近對齊界限的 kb 數（KB）。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束並出現錯誤。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的鏡像磁碟區，請在 [磁片 1] 和 [2] 上輸入：

```
create volume mirror size=1000 disk=1,2
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [建立命令](create.md)
