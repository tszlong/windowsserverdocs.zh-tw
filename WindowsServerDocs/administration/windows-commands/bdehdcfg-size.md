---
title: bdehdcfg 大小
description: Bdehdcfg size 命令的參考主題，它會在建立新的系統磁片磁碟機時指定系統磁碟分割的大小。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27662a46045a8701b40697198cddf1dcb1b51c1e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718617"
---
# <a name="bdehdcfg-size"></a>bdehdcfg：大小

指定建立新的系統磁片磁碟機時，系統磁碟分割的大小。 如果您未指定大小，此工具會使用預設值 300 MB。 系統磁片磁碟機的大小下限為 100 MB。 如果您要將系統修復或其他系統工具儲存在系統磁碟分割上，則應該相應增加大小。

> [!NOTE]
> **Size**命令無法與`target <drive_letter> merge`命令結合。

## <a name="syntax"></a>語法

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink} -size <size_in_mb>
```

#### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<size_in_mb>` | 指出要用於新資料分割的 mb 數。 |

## <a name="examples"></a>範例

若要將 500 MB 配置給預設系統磁片磁碟機：

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
