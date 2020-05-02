---
title: bdehdcfg newdriveletter
description: Bdehdcfg newdriveletter 命令的參考主題，它會將新的磁碟機號指派給用來做為系統磁片磁碟機的磁片磁碟機部分。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da09ae1469c6fc8370e6bd0f2f7a8f3efd8dc4f0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718670"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg： newdriveletter

將新的磁碟機號指派給用來做為系統磁片磁碟機的磁片磁碟機部分。 建議的最佳作法是不要將磁碟機號指派給您的系統磁片磁碟機。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -newdriveletter <drive_letter>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------| ----------- |
| `<drive_letter>` | 定義將指派給指定目標磁片磁碟機的磁碟機號。 |

## <a name="examples"></a>範例

若要指派預設磁片磁碟機磁碟機號`P`：

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
