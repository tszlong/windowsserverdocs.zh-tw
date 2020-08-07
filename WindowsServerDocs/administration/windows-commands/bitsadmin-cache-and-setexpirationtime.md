---
title: bitsadmin cache and setexpirationtime
description: Bitsadmin cache 和 setexpirationtime 命令的參考文章，其會設定快取到期時間。
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 367088525366707797296d844f134dcf8a02def7
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894740"
---
# <a name="bitsadmin-cache-and-setexpirationtime"></a>bitsadmin cache and setexpirationtime

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定快取到期時間。

## <a name="syntax"></a>語法

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| secs | 快取到期之前的秒數。 |

## <a name="examples"></a>範例

若要將快取設定為在60秒後過期：

```
bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
