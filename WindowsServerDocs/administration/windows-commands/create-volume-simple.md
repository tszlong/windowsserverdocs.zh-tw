---
title: create volume simple
description: Create volume simple 命令的參考文章，此命令會在指定的動態磁碟上建立簡單的磁片區。
ms.topic: reference
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 473ee42e996960af3e0f27579283114ca0b3e7f3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629064"
---
# <a name="create-volume-simple"></a>create volume simple

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在指定的動態磁碟上建立簡單磁片區。 建立磁碟區之後，焦點會自動移到新磁碟區。

## <a name="syntax"></a>語法

```
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>`  | 磁碟區大小以 MB 表示。 如果未指定大小，則新磁碟區會佔用磁碟上剩餘的空間。 |
| 磁片 =`<n>`  | 要在其上建立磁片區的動態磁碟。 如果未指定任何磁片，則會使用目前的磁片。 |
| align =`<n>` | 將所有磁片區範圍對齊到最接近的對齊界限。 通常會與硬體 RAID 邏輯單元編號搭配使用 (LUN) 陣列以改善效能。 `<n>` 這是從磁片開頭到最接近對齊界限的 kb (KB) 數目。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的磁片區，請在磁片1上輸入：

```
create volume simple size=1000 disk=1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [create 命令](create.md)
