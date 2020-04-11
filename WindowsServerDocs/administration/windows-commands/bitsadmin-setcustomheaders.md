---
title: bitsadmin setcustomheaders
description: 適用于**bitsadmin setcustomheaders**的 Windows 命令主題，它會將自訂 HTTP 標頭新增至 GET 要求。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5b1a28f03815a22a3f8d10b2c3d1d4a3a2ae635
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123020"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

將自訂 HTTP 標頭新增至傳送至 HTTP 伺服器的 GET 要求。

## <a name="syntax"></a>語法

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| `<header1> <header2>` 等 | 作業的自訂標頭。 |

## <a name="examples"></a>範例

下列範例會針對名為*myDownloadJob*的作業新增自訂 HTTP 標頭。 如需 GET 要求的詳細資訊，請參閱[方法定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3)和[標頭欄位定義](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)。

```
C:\>bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)