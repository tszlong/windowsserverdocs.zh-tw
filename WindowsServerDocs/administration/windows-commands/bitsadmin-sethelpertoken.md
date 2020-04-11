---
title: bitsadmin sethelpertoken
description: 適用于**bitsadmin sethelpertoken**的 Windows 命令主題，它會將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 helper token。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: ba4b9a4ed1b59d1b1aeda30353317739b7fdfa9e
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122987"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

將目前的命令提示字元主要權杖（或任意本機使用者帳戶的權杖，如果有指定）設定為 BITS 傳送作業的 [helper token](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)。

> [!NOTE]
> BITS 3.0 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /sethelpertoken <job> [<user_name@domain> <password>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| `<username@domain>` `<password>` | 選擇性。 要使用權杖的本機使用者帳號憑證。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
