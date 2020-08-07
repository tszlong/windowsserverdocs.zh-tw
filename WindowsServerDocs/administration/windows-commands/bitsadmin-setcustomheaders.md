---
title: bitsadmin setcustomheaders
description: Bitsadmin setcustomheaders 命令的參考文章，它會將自訂 HTTP 標頭新增至 GET 要求。
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 407e4aa85d8413167add716cd63fe620f73b0e12
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893188"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

將自訂 HTTP 標頭新增至傳送至 HTTP 伺服器的 GET 要求。 如需 GET 要求的詳細資訊，請參閱[方法定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3)和[標頭欄位定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)。

## <a name="syntax"></a>語法

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| `<header1> <header2>`以此類推 | 作業的自訂標頭。 |

## <a name="examples"></a>範例

若要為名為*myDownloadJob*的作業新增自訂 HTTP 標頭：

```
bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
