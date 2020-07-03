---
title: bitsadmin util and enableanalyticchannel
description: Bitsadmin util 和 enableanalyticchannel 命令的參考文章，可啟用或停用 BITS 用戶端分析通道。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 515402f42dee54baa662f37718841f70b1cd2882
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927412"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util and enableanalyticchannel

啟用或停用 BITS 用戶端分析通道。

## <a name="syntax"></a>Syntax

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| 參數 | 說明 |
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
