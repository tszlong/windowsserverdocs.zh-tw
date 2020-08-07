---
title: bitsadmin setmaxdownloadtime
description: Bitsadmin setmaxdownloadtime 命令的參考文章，其會設定下載超時（以秒為單位）。
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5453c476f304bce3564167da93518793d7eef043
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893112"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime

設定下載超時（以秒為單位）。

## <a name="syntax"></a>語法

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| timeout | 下載超時時間長度（以秒為單位）。 |

## <a name="examples"></a>範例

將名為*myDownloadJob*之作業的超時時間設定為10秒。

```
bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
