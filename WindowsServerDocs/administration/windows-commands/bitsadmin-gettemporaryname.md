---
title: bitsadmin gettemporaryname
description: Bitsadmin gettemporaryname 命令的參考文章，它會報告作業中指定檔案的暫存檔案。
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b5919d45f2b8497bb6e8fa6cf3650f49e27cd48
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893831"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname

報告作業中指定檔案的暫存檔案案。

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

若要針對名為*myDownloadJob*的作業報告檔案2的暫存檔案名：

```
bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
