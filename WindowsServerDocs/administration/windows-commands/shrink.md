---
title: shrink
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 09cc9ce7beebc04a0a45cd93c2b0da25c5a1a49e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441261"
---
# <a name="shrink"></a>shrink

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Diskpart 壓縮命令會將選取的磁碟區的大小減少您所指定的數量。 此命令會將可用磁碟空間可從磁碟區的結尾未使用的空間。

## <a name="syntax"></a>語法
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>參數

|  參數  |                                                                                             描述                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired=<n> |                                                     指定所需的空間量 （單位為 MB)，以減少磁碟區的大小由。                                                     |
| minimum=<n> |                                                           指定的最少的空間，以 mb 為單位來減少的磁碟區大小。                                                           |
|  querymax   |                       傳回最大空間量，以 mb 為單位的磁碟區可以降低。 如果應用程式目前正在存取磁碟區，這個值可能會變更。                        |
|   nowait    |                                                       強制命令立即壓縮程序仍在進行時傳回。                                                        |
|    noerr    | 針對僅限指令碼。 發生錯誤時，DiskPart 會繼續處理命令，如同未發生錯誤。 如果沒有這個參數，錯誤會造成 DiskPart 結束，錯誤碼。 |

## <a name="remarks"></a>備註
- 只有當格式化使用 NTFS 檔案系統，或它並沒有檔案系統，您可以減少磁碟區的大小。
- 此命令適用於基本磁碟區，以及簡單或跨距動態磁碟區。
- 如果未指定所需的數量，磁碟區會縮減的最少 （如果有指定）。
- 如果未指定最少，磁碟區會縮減所需的數量 （如果有指定）。
- 如果指定不以最短，也不想要的數量，則磁碟區會縮減盡量。
- 如果已指定最短，但沒有足夠的可用空間，命令會失敗。
- 這項作業成功，就必須選取磁碟區。 使用**選取磁碟區**命令來選取磁碟區，並將焦點移到它。
- 此命令無法運作原始設備製造商 (OEM) 的資料分割、 可延伸韌體介面 (EFI) 系統磁碟分割或修復磁碟分割上。
  ## <a name="BKMK_examples"></a>範例
  若要選取的磁碟區大小縮減 250 和 500 mb 之間最大的可能量，輸入：
  ```
  shrink desired=500 minimum=250
  ```
  若要顯示的磁碟區可以縮減的 MB 的最大數目，請輸入：
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
