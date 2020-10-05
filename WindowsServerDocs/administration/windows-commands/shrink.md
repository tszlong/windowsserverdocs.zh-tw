---
title: shrink
description: DiskPart shrink 命令的參考文章，此命令會根據您指定的數量減少所選磁片區的大小。
ms.topic: reference
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8b03c006243f263a4f1c7fe991580d5e74f9a7a7
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718315"
---
# <a name="shrink"></a>shrink

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart shrink 命令會依據您指定的數量，減少所選磁片區的大小。 此命令可讓磁片區尾端未使用的空間取得可用磁碟空間。

必須選取一個磁碟區，這項操作才能繼續。 使用 [ **選取磁片** 區] 命令來選取磁片區，並將焦點移至該磁片區。

> [!NOTE]
> 這個命令適用於基本磁碟區，以及簡單或跨距動態磁碟區。 它不適用於原始設備製造商 (OEM) 磁碟分割、可延伸的固件介面 (EFI) 系統磁碟分割或復原磁碟分割。

## <a name="syntax"></a>語法

```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 需要 =`<n>` | 指定要縮減磁碟區大小的空間量，以 MB 為單位。 |
| 最小值 =`<n>` | 指定要縮減磁碟區大小的最小空間量，以 MB 為單位。 |
| querymax | 傳回磁片區可縮減的最大空間量（以 MB 為單位）。 如果應用程式目前正在存取磁碟區，這個值可能會變化。 |
| nowait | 在壓縮程序仍然進行中時，強制命令立即傳回。 |
| noerr | 僅適合執行指令。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

#### <a name="remarks"></a>備註

- 唯有磁碟區是使用 NTFS 檔案系統格式化，或者沒有檔案系統時，才能夠縮減磁碟區的大小。

- 如果未指定所需的數量，則磁片區會依最小數量 (（如果指定了) ）。

- 如果未指定最小數量，則磁片區會根據所需的數量 (（如果指定) ）。

- 如果未指定最小數量或所需數量，則磁片區會盡可能減少。

- 如果指定了最小數量，但沒有足夠的可用空間，則命令會失敗。

## <a name="examples"></a>範例

若要減少所選磁片區的大小（以250到 500 mb 之間的最大可能數量），請輸入：

```
shrink desired=500 minimum=250
```

若要顯示可減少磁片區的最大 MB 數，請輸入：

```
shrink querymax
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [Resize-Partition](/powershell/module/storage/resize-partition?view=win10-ps&preserve-view=true)
