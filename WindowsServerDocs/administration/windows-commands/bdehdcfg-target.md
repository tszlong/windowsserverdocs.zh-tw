---
title: bdehdcfg target
description: Bdehdcfg target 命令的參考文章，它會準備磁碟分割，以供 BitLocker 和 Windows 復原作為系統磁片磁碟機。
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4da764bf25a661c53c27b15cbbee8e4d4ec2981
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895037"
---
# <a name="bdehdcfg-target"></a>bdehdcfg：目標

準備磁碟分割，以供 BitLocker 和 Windows 復原作為系統磁片磁碟機。 預設會建立此分割區，但不含磁碟機號。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| default | 表示命令列工具會遵循與 BitLocker 安裝程式嚮導相同的進程。 |
| 空間 | 從磁片上可用的未配置空間建立系統磁碟分割。 |
| `<drive_letter>`排 | 減少建立使用中系統磁碟分割所需數量所指定的磁片磁碟機。 若要使用此命令，指定的磁片磁碟機至少必須有5% 的可用空間。 |
| `<drive_letter>`merge | 使用指定為作用中系統磁碟分割的磁片磁碟機。 作業系統磁片磁碟機不可以是合併的目標。 |

## <a name="examples"></a>範例

若要指定現有的磁片磁碟機 (P) 作為系統磁片磁碟機：

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
