---
title: create volume simple
description: Create volume simple 命令的參考文章，它會在指定的動態磁碟上建立簡單磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e7924ca4394a64fd5a8d92577a3f3f62b917461
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929515"
---
# <a name="create-volume-simple"></a>create volume simple

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在指定的動態磁碟上建立簡單磁片區。 建立磁碟區之後，焦點會自動移到新磁碟區。

## <a name="syntax"></a>語法

```
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| 大小 =`<n>`  | 磁碟區大小以 MB 表示。 如果未指定大小，則新磁碟區會佔用磁碟上剩餘的空間。 |
| 磁片 =`<n>`  | 要在其上建立磁片區的動態磁碟。 如果未指定任何磁片，則會使用目前的磁片。 |
| align =`<n>` | 將所有磁片區範圍對齊最接近的對齊界限。 通常與硬體 RAID 邏輯單元編號（LUN）陣列搭配使用，以改善效能。 `<n>`這是從磁片開始到最接近對齊界限的 kb 數（KB）。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的磁片區，請在 disk 1 上輸入：

```
create volume simple size=1000 disk=1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [建立命令](create.md)
