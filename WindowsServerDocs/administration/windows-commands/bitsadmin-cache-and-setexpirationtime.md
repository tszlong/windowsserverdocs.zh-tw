---
title: bitsadmin cache and setexpirationtime
description: Bitsadmin cache and setexpirationtime 命令的參考文章，會設定快取到期時間。
ms.topic: reference
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e07e862d8577c33daec24bbc93fe5859f13944db
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028806"
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

若要將快取設定為在60秒內過期：

```
bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
