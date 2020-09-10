---
title: bdehdcfg size
description: Bdehdcfg size 命令的參考文章，這個命令會在建立新的系統磁片磁碟機時，指定系統磁碟分割的大小。
ms.topic: reference
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c68322fe999a39c4a2913826f520f0b401a9d0ac
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632824"
---
# <a name="bdehdcfg-size"></a>bdehdcfg：大小

指定在建立新的系統磁片磁碟機時，系統磁碟分割的大小。 如果您未指定大小，此工具將會使用預設值 300 MB。 系統磁片磁碟機的大小下限為 100 MB。 如果您要將系統修復或其他系統工具儲存在系統磁碟分割上，您應該相應增加大小。

> [!NOTE]
> **Size**命令無法與 `target <drive_letter> merge` 命令結合。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink} -size <size_in_mb>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<size_in_mb>` | 指出要用於新分割區的 mb (MB) 數目。 |

## <a name="examples"></a>範例

若要將 500 MB 配置到預設系統磁片磁碟機：

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
