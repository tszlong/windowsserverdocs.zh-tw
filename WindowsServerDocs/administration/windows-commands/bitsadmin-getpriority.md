---
title: bitsadmin getpriority
description: Bitsadmin getpriority 命令的參考文章，此命令會抓取指定作業的優先順序。
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 2aeff973b0ca285cc8c9852f284e314879f8de02
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028696"
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
