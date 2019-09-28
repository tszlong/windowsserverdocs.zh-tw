---
title: shrink
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0e5caa0103018c94671d7441a6d2349ab734be6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370897"
---
# <a name="shrink"></a>shrink

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Diskpart shrink 命令會依您指定的數量減少所選磁片區的大小。 此命令可從磁片區結尾的未使用空間中取得可用的磁碟空間。

## <a name="syntax"></a>語法
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>參數

|  參數  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired = <n> |                                                     指定要縮減磁片區大小所需的空間量（以 mb 為單位）。                                                     |
| 最小值 = <n> |                                                           指定要縮減磁片區大小的最小空間量（以 MB 為單位）。                                                           |
|  querymax   |                       傳回磁片區可縮減的最大空間量（以 MB 為單位）。 如果應用程式目前正在存取磁片區，這個值可能會變更。                        |
|   nowait    |                                                       當壓縮進程仍在進行中時，強制命令立即傳回。                                                        |
|    noerr    | 僅適用于腳本。 當發生錯誤時，DiskPart 會繼續處理命令，就像未發生錯誤一樣。 若沒有此參數，錯誤會導致 DiskPart 結束，錯誤碼為。 |

## <a name="remarks"></a>備註
- 只有當磁片區是使用 NTFS 檔案系統進行格式化，或如果沒有檔案系統時，您才可以縮減磁片區的大小。
- 此命令適用于基本磁碟區，以及簡單或跨距動態磁碟區上的。
- 如果未指定所需的數量，磁片區將會縮減為最小數量（若有指定）。
- 如果未指定最小金額，磁片區將會降低所需的數量（若有指定）。
- 如果未指定最小金額或所需的數量，則會盡可能減少磁片區。
- 如果指定了最小金額，但沒有足夠的可用空間，則此命令將會失敗。
- 必須選取磁片區，此操作才能成功。 使用 [**選取磁片**區] 命令來選取磁片區，並將焦點移至該磁片區。
- 此命令不會在原始設備製造商（OEM）磁碟分割、可延伸韌體介面（EFI）系統磁碟分割或復原磁碟分割上操作。
  ## <a name="BKMK_examples"></a>典型
  若要減少所選磁片區的大小（以250到 500 mb 的最大可能數量為限），請輸入：
  ```
  shrink desired=500 minimum=250
  ```
  若要顯示可減少磁片區的 MB 數上限，請輸入：
  ```
  shrink querymax
  ```

[調整大小-資料分割](https://technet.microsoft.com/library/hh848680.aspx)
