---
title: create volume raid
description: 建立磁片區 raid 命令的參考文章，此命令會使用三個或多個指定的動態磁碟來建立 RAID-5 磁片區。
ms.topic: reference
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c936205f6132a8a2bc06e455dd07761715df684f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033146"
---
# <a name="create-volume-raid"></a>create volume raid

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用三個或多個指定的動態磁碟來建立 RAID-5 磁片區。 建立磁碟區之後，焦點會自動移到新磁碟區。

## <a name="syntax"></a>語法

```
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 磁碟區佔據每個磁碟上的磁碟空間 (以 MB 為單位)。 如果未指定大小，將會建立最大可能的 RAID-5 磁片區。 具有最小可用連續剩餘空間的磁碟決定 RAID-5 磁碟區大小，並會從每個磁碟配置相同數量的空間。 因為同位檢查需要部分磁碟空間，所以 RAID-5 磁碟區中可用磁碟空間的實際數量小於磁碟空間的總數量。 |
| 磁片 =`<n>,<n>,<n>[,<n>,...]` | 要在其上建立 RAID-5 磁片區的動態磁碟。 您至少需要三個動態磁碟以建立一個 RAID-5 磁碟區。 `size=<n>`在每個磁片上都會配置等於的空間量。 |
| align =`<n>` | 將所有磁片區範圍對齊到最接近的對齊界限。 通常會與硬體 RAID 邏輯單元編號搭配使用 (LUN) 陣列以改善效能。 `<n>` 這是從磁片開頭到最接近對齊界限的 kb (KB) 數目。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的 RAID-5 磁片區，使用磁片1、2和3，請輸入：

```
create volume raid size=1000 disk=1,2,3
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [create 命令](create.md)
