---
title: bitsadmin getbytestransferred
description: Bitsadmin getbytestransferred 命令的參考文章，此命令會抓取針對指定工作傳輸的位元組數目。
ms.topic: reference
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67e1432f2fbef32ac47320d87993f5d62053bb71
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028736"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

抓取針對指定工作傳輸的位元組數目。

## <a name="syntax"></a>語法

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出針對名為 *myDownloadJob*的作業所傳送的位元組數目：

```
bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
