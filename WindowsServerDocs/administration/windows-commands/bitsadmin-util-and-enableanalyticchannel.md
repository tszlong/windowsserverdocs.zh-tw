---
title: bitsadmin util and enableanalyticchannel
description: Bitsadmin util 和 enableanalyticchannel 命令的參考主題，可啟用或停用 BITS 用戶端分析通道。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f5c8c924d1011928aca6ec1bcebd4d71abb015
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707765"
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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
