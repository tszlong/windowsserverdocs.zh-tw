---
title: bitsadmin sethelpertoken
description: Bitsadmin sethelpertoken 命令的參考文章，如果指定) 作為 BITS 傳送工作的協助程式權杖，則會將目前命令提示字元的主要權杖 (或任意本機使用者帳戶的權杖。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: a691d010aa112f1ac609077be6a6968f79d73c72
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630909"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

如果指定) 作為 BITS 傳送工作的協助程式 [權杖](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)，則設定目前命令提示字元的主要權杖 (或任意本機使用者帳戶的權杖。

> [!NOTE]
> BITS 3.0 及更早版本不支援此命令。

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
