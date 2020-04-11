---
title: bitsadmin util 和 enableanalyticchannel
description: 適用于**bitsadmin util 和 enableanalyticchannel**的 Windows 命令主題，可啟用或停用 BITS 用戶端分析通道。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0d7645aa-b91b-4ed7-b630-a1e1be6f6ae9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ff1f835415979036fdc0f8aa637fe693e57d46
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122688"
---
# <a name="bitsadmin-util-and-enableanalyticchannel"></a>bitsadmin util 和 enableanalyticchannel

啟用或停用 BITS 用戶端分析通道。

## <a name="syntax"></a>語法

```
bitsadmin /util /enableanalyticchannel TRUE|FALSE
```

| 參數 | 描述 |
| --------- | ---------- |
| TRUE 或 FALSE | **TRUE**會開啟指定檔案的內容驗證，而**FALSE**則關閉它。 |

## <a name="examples"></a>範例

下列範例會啟用 BITS 用戶端分析通道。

```
C:\>bitsadmin /util / enableanalyticchannel TRUE
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)