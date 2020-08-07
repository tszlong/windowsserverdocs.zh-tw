---
title: bitsadmin getcustomheaders
description: Bitsadmin getcustomheaders 命令的參考文章，它會從作業中抓取自訂 HTTP 標頭。
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3cc970a7aeee1be678a3c77d852af91ae0ab98c8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894419"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders

從作業中抓取自訂 HTTP 標頭。

## <a name="syntax"></a>語法

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取得名為*myDownloadJob*之作業的自訂標頭：

```
bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
