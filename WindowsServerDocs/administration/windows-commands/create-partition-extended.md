---
title: create partition extended
description: 建立分割區擴充命令的參考文章，此命令會在磁片上建立具有焦點的延伸磁碟分割。
ms.topic: reference
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b04e9ad75161dbde02046ad0a0c5392b19473fa9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629195"
---
# <a name="create-partition-extended"></a>create partition extended

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在磁片上建立具有焦點的延伸磁碟分割。 建立磁碟分割之後，焦點會自動移到新磁碟分割。

>[!IMPORTANT]
> 您只能將此命令用於主開機記錄 (MBR) 磁片。 您必須使用 [ [選取磁片](select-disk.md) ] 命令來選取基本的 MBR 磁碟，並將焦點移至該磁片。
>
> 在建立邏輯磁碟機之前，必須建立延伸磁碟分割。 每個磁碟僅能建立一個延伸磁碟分割。 如果您嘗試在另一個延伸磁碟分割中建立延伸磁碟分割，則此命令會失敗。

## <a name="syntax"></a>語法

```
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 大小 =`<n>` | 指定磁碟分割大小為 MB。 如果未指定大小，則磁碟分割會繼續，直到延伸磁碟分割中的可用空間不足為止。 |
| offset =`<n>` | 以 kb 為單位，指定要在其中建立資料分割的位移（以 kb 為單位） (KB) 。 如果沒有指定位移，磁碟分割就會從磁片上可用空間的開頭開始，該磁片夠大，足以容納新的磁碟分割。 |
| align =`<n>` | 將所有資料分割範圍對齊到最接近的對齊界限。 通常會與硬體 RAID 邏輯單元編號搭配使用 (LUN) 陣列以改善效能。 `<n>` 這是從磁片開頭到最接近對齊界限的 kb (KB) 數目。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="examples"></a>範例

若要建立大小為 1000 mb 的延伸磁碟分割，請輸入：

```
create partition extended size=1000
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [create 命令](create.md)

- [select disk](select-disk.md)
