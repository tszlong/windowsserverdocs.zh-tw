---
title: shrink
description: DiskPart 的參考文章壓縮，可依您指定的數量減少所選磁片區的大小。
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccae64b5f54c197f8eb1cd684a74c44945b3d069
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882379"
---
# <a name="shrink"></a>shrink

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart shrink 命令會依您指定的數量減少所選磁片區的大小。 此命令可從磁片區結尾的未使用空間中取得可用的磁碟空間。

## <a name="syntax"></a>語法
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
### <a name="parameters"></a>參數

|  參數  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired =<n> |                                                     指定要縮減磁碟區大小的空間量，以 MB 為單位。                                                     |
| 最小值 =<n> |                                                           指定要縮減磁碟區大小的最小空間量，以 MB 為單位。                                                           |
|  querymax   |                       傳回磁片區可縮減的最大空間量（以 MB 為單位）。 如果應用程式目前正在存取磁碟區，這個值可能會變化。                        |
|   nowait    |                                                       當壓縮進程仍在進行中時，強制命令立即傳回。                                                        |
|    noerr    | 僅適用于腳本。 遇到錯誤時，DiskPart 會像沒有發生錯誤一般繼續處理命令。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="remarks"></a>備註
- 唯有磁碟區是使用 NTFS 檔案系統格式化，或者沒有檔案系統時，才能夠縮減磁碟區的大小。
- 這個命令適用於基本磁碟區，以及簡單或跨距動態磁碟區。
- 如果未指定所需的數量，則磁片區將會因為指定) 而縮減 (的最小數量。
- 如果未指定最小金額，磁片區將會降低所需的數量， (如果指定) 。
- 如果未指定最小金額或所需的數量，則會盡可能減少磁片區。
- 如果指定了最小金額，但沒有足夠的可用空間，則此命令將會失敗。
- 必須選取一個磁碟區，這項操作才能繼續。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。
- 此命令不會 (OEM) 磁碟分割、可延伸固件介面 (EFI) 系統磁碟分割或修復磁碟分割的原始設備製造商操作。
  ## <a name="examples"></a>範例
  若要減少所選磁片區的大小（以250到 500 mb 的最大可能數量為限），請輸入：
  ```
  shrink desired=500 minimum=250
  ```
  若要顯示可減少磁片區的 MB 數上限，請輸入：
  ```
  shrink querymax
  ```

[Resize-Partition](/powershell/module/storage/resize-partition?view=win10-ps)
