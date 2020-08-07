---
title: bitsadmin geterror
description: Bitsadmin geterror 命令的參考文章，它會抓取所指定工作的詳細錯誤資訊。
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae0b2391267ddc1508d8343b909b9739a0e01ffb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894357"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

抓取指定之作業的詳細錯誤資訊。

## <a name="syntax"></a>語法

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的錯誤資訊：

```
bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
