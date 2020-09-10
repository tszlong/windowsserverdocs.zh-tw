---
title: bdehdcfg newdriveletter
description: Bdehdcfg newdriveletter 命令的參考文章，此命令會將新的磁碟機號指派給用來作為系統磁片磁碟機的磁片磁碟機部分。
ms.topic: reference
ms.assetid: f1f200a0-6850-4f0d-9047-f9f982a590f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5ac14224b55782e8e36cfe156a900e1614bd118d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632880"
---
# <a name="bdehdcfg-newdriveletter"></a>bdehdcfg： newdriveletter

將新的磁碟機號指派給用來作為系統磁片磁碟機的磁片磁碟機部分。 建議的最佳作法是不要將磁碟機號指派給您的系統磁片磁碟機。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -newdriveletter <drive_letter>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------| ----------- |
| `<drive_letter>` | 定義將指派給指定之目標磁片磁碟機的磁碟機號。 |

## <a name="examples"></a>範例

若要指派預設磁片磁碟機磁碟機號 `P` ：

```
bdehdcfg -target default -newdriveletter P:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
