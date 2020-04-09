---
title: bdehdcfg 目標
description: 適用于**bdehdcfg target**的 Windows 命令主題，它會準備磁碟分割，以供 BitLocker 和 Windows 復原作為系統磁片磁碟機。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0f3e90fbb8725360cf8db335e79721e2328ab3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851011"
---
# <a name="bdehdcfg-target"></a>bdehdcfg：目標

準備磁碟分割，以供 BitLocker 和 Windows 復原作為系統磁片磁碟機。 預設會建立此分割區，但不含磁碟機號。

如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| default | 表示命令列工具會遵循與 BitLocker 安裝程式嚮導相同的進程。 |
| 空間 | 從磁片上可用的未配置空間建立系統磁碟分割。 |
| `<DriveLetter>` 壓縮 | 減少建立使用中系統磁碟分割所需數量所指定的磁片磁碟機。 若要使用此命令，指定的磁片磁碟機至少必須有5% 的可用空間。 |
| `<DriveLetter>` 合併 | 使用指定為作用中系統磁碟分割的磁片磁碟機。 作業系統磁片磁碟機不可以是合併的目標。 |

## <a name="examples"></a><a name=BKMK_Examples></a>典型

下列範例說明如何使用**target**命令，將現有的磁片磁碟機（P）指定為系統磁片磁碟機。

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Bdehdcfg](bdehdcfg.md)