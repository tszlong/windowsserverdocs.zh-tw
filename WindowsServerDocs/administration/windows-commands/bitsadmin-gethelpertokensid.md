---
title: bitsadmin gethelpertokensid
description: Bitsadmin gethelpertokensid 命令的參考文章，它會傳回 BITS 傳送作業的協助程式權杖 SID （如果有設定的話）。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 3c8ef7502c524994454c697e2fa7d5dee223da5d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955460"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

傳回 BITS 傳送作業的協助程式 [權杖](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)SID （如果已設定的話）。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |

## <a name="examples"></a>範例

若要取出名為*myDownloadJob*的 BITS 傳送工作的 SID：

```
bitsadmin /gethelpertokensid myDownloadJob
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
