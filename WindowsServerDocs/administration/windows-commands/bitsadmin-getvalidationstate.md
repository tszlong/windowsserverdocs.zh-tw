---
title: bitsadmin getvalidationstate
description: Bitsadmin getvalidationstate 命令的參考文章，它會報告作業中指定檔案的內容驗證狀態。
ms.topic: reference
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 173bbaad6508ec3ae8100232fda598fe7a422296
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028636"
---
# <a name="bitsadmin-getvalidationstate"></a>bitsadmin getvalidationstate

報告作業中指定檔案的內容驗證狀態。

## <a name="syntax"></a>語法

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| file_index | 從0開始。 |

## <a name="examples"></a>範例

若要在名為 *myDownloadJob*的作業中取出檔案2的內容驗證狀態：

```
bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
