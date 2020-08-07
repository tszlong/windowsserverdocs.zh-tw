---
title: bitsadmin util and version
description: Bitsadmin util 和 version 命令的參考文章，其會顯示 BITS 服務的版本。
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3490fe4e3eaa217b81287d8a2ed38a6fdb98279
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880789"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util and version

顯示 BITS 服務的版本 (例如 2.0) 。

> [!NOTE]
> BITS 1.5 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /verbose | 使用此參數可顯示每個位相關 DLL 的檔案版本，並確認 BITS 服務是否可以啟動。|

## <a name="examples"></a>範例

以顯示 BITS 服務的版本。

```
bitsadmin /util /version
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
