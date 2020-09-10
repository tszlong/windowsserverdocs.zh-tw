---
title: bitsadmin util and enableanalyticchannel
description: Bitsadmin util and enableanalyticchannel 命令的參考文章，可啟用或停用 BITS 用戶端分析通道。
ms.topic: reference
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 391b53694fcb21d1c17085d487908b090ad88f0a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630565"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util and enableanalyticchannel

啟用或停用 BITS 用戶端分析通道。

## <a name="syntax"></a>語法

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| 參數 | 描述 |
| --------- | ---------- |
| TRUE 或 FALSE | **TRUE** 會開啟指定檔案的內容驗證，而 **FALSE** 則會關閉它。 |

## <a name="examples"></a>範例

開啟或關閉 BITS 用戶端分析通道。

```
bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
