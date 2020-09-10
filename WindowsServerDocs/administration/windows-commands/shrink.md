---
title: shrink
description: DiskPart 壓縮的參考文章，可根據您指定的數量減少所選磁片區的大小。
ms.topic: reference
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2fdfc8ba34f4e91bfafa1f8bf5341f9e4186cb5d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640951"
---
# <a name="shrink"></a>shrink

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart shrink 命令會依據您指定的數量，減少所選磁片區的大小。 此命令可讓磁片區尾端未使用的空間取得可用磁碟空間。

## <a name="syntax"></a>語法
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
### <a name="parameters"></a>參數

|  參數  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 需要 =<n> |                                                     指定要縮減磁碟區大小的空間量，以 MB 為單位。                                                     |
| 最小值 =<n> |                                                           指定要縮減磁碟區大小的最小空間量，以 MB 為單位。                                                           |
|  querymax   |                       傳回磁片區可縮減的最大空間量（以 MB 為單位）。 如果應用程式目前正在存取磁碟區，這個值可能會變化。                        |
|   nowait    |                                                       當壓縮程式仍在進行中時，強制命令立即傳回。                                                        |
|    noerr    | 僅供腳本之用。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 如果沒有這個參數，錯誤會導致 DiskPart 結束，並產生錯誤碼。 |

## <a name="remarks"></a>備註
- 唯有磁碟區是使用 NTFS 檔案系統格式化，或者沒有檔案系統時，才能夠縮減磁碟區的大小。
- 這個命令適用於基本磁碟區，以及簡單或跨距動態磁碟區。
- 如果未指定所需的數量，則磁片區將會依最小數量 (（如果指定了) ）。
- 如果未指定最小數量，則磁片區將會依據所需的數量 (（如果指定) ）。
- 如果未指定最小數量或所需數量，則磁片區會盡可能減少。
- 如果指定了最小數量，但沒有足夠的可用空間，則命令會失敗。
- 必須選取一個磁碟區，這項操作才能繼續。 使用 [ **選取磁片** 區] 命令來選取磁片區，並將焦點移至該磁片區。
- 此命令不適用於原始設備製造商 (OEM) 磁碟分割、可擴充的固件介面 (EFI) 系統磁碟分割或復原磁碟分割。
  ## <a name="examples"></a>範例
  若要減少所選磁片區的大小（以250到 500 mb 之間的最大可能數量），請輸入：
  ```
  shrink desired=500 minimum=250
  ```
  若要顯示可減少磁片區的最大 MB 數，請輸入：
  ```
  shrink querymax
  ```

[Resize-Partition](/powershell/module/storage/resize-partition?view=win10-ps)
