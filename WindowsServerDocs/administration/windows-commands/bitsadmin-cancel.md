---
title: bitsadmin cancel
description: Bitsadmin cancel 命令的參考文章，它會從傳送佇列中移除作業，並刪除與作業相關聯的所有暫存檔案。
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 603a398aef42052e25828b917748ed2b10287620
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894646"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

從傳輸佇列中移除作業，並刪除與作業相關聯的所有暫存檔案。

## <a name="syntax"></a>語法

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要從傳送佇列中移除*myDownloadJob*作業：

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
