---
title: bdehdcfg target
description: Bdehdcfg 目標命令的參考文章，此命令會準備磁碟分割，以供 BitLocker 和 Windows 修復用來作為系統磁片磁碟機。
ms.topic: reference
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7cc13e3c224aa1af944c5e9c9550737addcb6980
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632809"
---
# <a name="bdehdcfg-target"></a>bdehdcfg：目標

準備磁碟分割，以供 BitLocker 和 Windows 修復作為系統磁片磁碟機。 依預設，會建立不含磁碟機號的磁碟分割。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| default | 指出命令列工具將遵循與 BitLocker 安裝 wizard 相同的程式。 |
| 未配置 | 從磁片上可用的未配置空間建立系統磁碟分割。 |
| `<drive_letter>` 收縮 | 減少建立作用中系統分割區所需的磁片磁碟機數目。 若要使用此命令，指定的磁片磁碟機至少必須有5% 的可用空間。 |
| `<drive_letter>` 合併 | 使用指定為使用中系統磁碟分割的磁片磁碟機。 作業系統磁片磁碟機不能是合併的目標。 |

## <a name="examples"></a>範例

若要指定現有磁片磁碟機 (P) 作為系統磁片磁碟機：

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
