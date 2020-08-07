---
title: bitsadmin cache 和 deleteURL
description: Bitsadmin cache 和 deleteURL 命令的參考文章，它會刪除指定 URL 的所有快取專案。
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1a21a1994711e2548e9e08094f88f46edafe481
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894840"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>bitsadmin cache 和 deleteURL

刪除指定 URL 的所有快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /deleteURL URL
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| URL | 識別遠端檔案的統一資源定位器。 |

## <a name="examples"></a>範例

若要刪除的所有快取專案 `https://www.contoso.com/en/us/default.aspx` ：

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
