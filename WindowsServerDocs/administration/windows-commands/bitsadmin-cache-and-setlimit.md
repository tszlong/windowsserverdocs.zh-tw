---
title: bitsadmin cache and setlimit
description: Bitsadmin cache 和 setlimit 命令的參考文章，其會設定快取大小限制。
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41a1331a19f66e7d84dc3eb57b04d42596a40628
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894707"
---
# <a name="bitsadmin-cache-and-setlimit"></a>bitsadmin cache and setlimit

設定快取大小限制。

## <a name="syntax"></a>語法

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| percent | 快取限制定義為總硬碟空間的百分比。 |

## <a name="examples"></a>範例

若要將快取大小限制設定為50%：

```
bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
