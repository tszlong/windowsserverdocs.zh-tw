---
title: bitsadmin resume
description: Bitsadmin resume 命令的參考文章，它會在傳送佇列中啟用新的或已暫止的工作。
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a18bd6c0a69ff4b366f66d34ec472be9aaeecba2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893303"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

在傳送佇列中啟用新的或已暫止的工作。 如果您不小心地繼續作業，或只需要暫停作業，您可以使用[bitsadmin 暫](bitsadmin-suspend.md)止參數來暫停作業。

## <a name="syntax"></a>語法

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要繼續名為*myDownloadJob*的作業：

```
bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 暫止命令](bitsadmin-suspend.md)

- [bitsadmin 命令](bitsadmin.md)
