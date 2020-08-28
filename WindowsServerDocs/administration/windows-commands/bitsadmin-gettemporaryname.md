---
title: bitsadmin gettemporaryname
description: Bitsadmin gettemporaryname 命令的參考文章，它會報告作業中指定檔案的暫存檔案名。
ms.topic: reference
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9e03b00fa1885b89f25dab2781ce988aa0c5df8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89034806"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname

報告作業中指定檔案的暫存檔案名。

## <a name="syntax"></a>語法

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| file_index | 從0開始。 |

## <a name="examples"></a>範例

若要針對名為 *myDownloadJob*的作業報告檔2的暫存檔案名：

```
bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
