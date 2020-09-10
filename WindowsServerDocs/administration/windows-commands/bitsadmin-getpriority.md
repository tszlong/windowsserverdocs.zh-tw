---
title: bitsadmin getpriority
description: Bitsadmin getpriority 命令的參考文章，此命令會抓取指定作業的優先順序。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 397a762a210aeae7a02e49283330a2d4214876e6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631763"
---
# <a name="bitsadmin-getpriority"></a>bitsadmin getpriority

抓取指定作業的優先順序。

## <a name="syntax"></a>語法

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

#### <a name="output"></a>輸出

此命令傳回的優先順序可以是：

- **前景**

- **高**

- **正常**

- **低**

- **UNKNOWN**

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*之作業的優先順序：

```
bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
