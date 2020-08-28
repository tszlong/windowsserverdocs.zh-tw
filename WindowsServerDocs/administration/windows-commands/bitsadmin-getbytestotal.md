---
title: bitsadmin getbytestotal
description: Bitsadmin getbytestotal 命令的參考文章，此命令會抓取指定作業的大小。
ms.topic: reference
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a8496234c0907061997ddd790e03f78d84c5380
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024422"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

抓取指定作業的大小。

## <a name="syntax"></a>語法

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為 *myDownloadJob*的作業大小：

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
