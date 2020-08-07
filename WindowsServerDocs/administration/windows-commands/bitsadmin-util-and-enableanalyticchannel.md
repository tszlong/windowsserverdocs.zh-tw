---
title: bitsadmin util and enableanalyticchannel
description: Bitsadmin util 和 enableanalyticchannel 命令的參考文章，可啟用或停用 BITS 用戶端分析通道。
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c4577f635444c1830e232e1baeb12fac8c75476
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880956"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util and enableanalyticchannel

啟用或停用 BITS 用戶端分析通道。

## <a name="syntax"></a>語法

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| 參數 | 描述 |
| --------- | ---------- |
| TRUE 或 FALSE | **TRUE**會開啟指定檔案的內容驗證，而**FALSE**則關閉它。 |

## <a name="examples"></a>範例

開啟或關閉 BITS 用戶端分析通道。

```
bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
