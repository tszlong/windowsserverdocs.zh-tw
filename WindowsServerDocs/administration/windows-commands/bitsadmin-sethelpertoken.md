---
title: bitsadmin sethelpertoken
description: Bitsadmin sethelpertoken 命令的參考文章，它會將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 helper token。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: e9fe469b8ae4a8a553245b1e22d48ee2cacf23e3
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955700"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 [helper token](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /sethelpertoken <job> [<user_name@domain> <password>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| `<username@domain>` `<password>` | 選擇性。 要使用權杖的本機使用者帳號憑證。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
