---
title: bitsadmin util and version
description: Bitsadmin util and version 命令的參考文章，其中顯示 BITS 服務的版本。
ms.topic: reference
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 783b709840070847c90bbf9d2b4aebc0758a5742
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630409"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util and version

顯示 BITS 服務的版本 (例如 2.0) 。

> [!NOTE]
> BITS 1.5 及更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /verbose | 您可以使用此參數來顯示每個位相關 DLL 的檔案版本，並確認 BITS 服務是否可以啟動。|

## <a name="examples"></a>範例

以顯示 BITS 服務的版本。

```
bitsadmin /util /version
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
