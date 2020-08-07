---
title: bitsadmin sethelpertoken
description: Bitsadmin sethelpertoken 命令的參考文章，其會設定目前命令提示字元的主要權杖 (或任意本機使用者帳戶的權杖（如果指定) 做為 BITS 傳送作業的協助程式權杖）。
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 15d0288919b16c038c3b310b6ea42c184b11b5a8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893152"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

如果指定) 做為 BITS 傳送作業的協助程式 [權杖](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)，則設定目前命令提示字元的主要權杖 (或任意本機使用者帳戶的權杖。

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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
