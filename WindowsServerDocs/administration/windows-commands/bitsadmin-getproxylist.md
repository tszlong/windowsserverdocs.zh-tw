---
title: bitsadmin getproxylist-抓取指定之作業的 proxy 清單。
description: Bitsadmin getproxylist 命令的參考文章，它會抓取指定工作的 proxy 清單。
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bcd94c65a11006a795f071224397d8b3081b7548
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893980"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

抓取要用於指定作業的 proxy 伺服器清單（以逗號分隔）。

## <a name="syntax"></a>語法

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的 proxy 清單：

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
