---
title: 作用中
description: '**Active**的 Windows 命令主題（在基本磁碟上）會將具有焦點的磁碟分割標示為作用中。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42f2e0d367344355e8f9a570f37cfbdc5dfc4590
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851371"
---
# <a name="active"></a>作用中

在基本磁碟上，將帶有焦點的磁碟分割標記為啟動。

> [!CAUTION]
> DiskPart 只會驗證磁碟分割是否能夠包含作業系統啟動檔案。 DiskPart 不檢查磁碟分割的內容。 如果您不小心將磁碟分割標示為使用中，而且它不包含作業系統啟動檔案，則您的電腦可能無法啟動。

## <a name="syntax"></a>語法

```
active
```- 

## Remarks

-   This informs the basic input/output system (BIOS) or Extensible Firmware Interface (EFI) that the partition or volume is a valid system partition or system volume.

-   Only partitions can be marked as active.

-   A partition must be selected for this operation to succeed. Use the **select partition** command to select a partition and shift the focus to it.

## <a name=BKMK_examples></a>Examples

To mark the partition with focus as the active partition, type:

```
作用中
```
## Additional References

- [Command-Line Syntax Key](command-line-syntax-key.md)