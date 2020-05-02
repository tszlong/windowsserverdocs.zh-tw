---
title: bitsadmin cache and setlimit
description: Bitsadmin cache 和 setlimit 命令的參考主題，其會設定快取大小限制。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4c41102bfb87ff6d48113c4e85a821b821b5b01
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718282"
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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
