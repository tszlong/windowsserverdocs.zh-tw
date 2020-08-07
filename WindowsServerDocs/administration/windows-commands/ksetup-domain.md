---
title: ksetup domain
description: Ksetup 網域命令的參考文章，其會設定所有 Kerberos 作業的功能變數名稱。
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81135ed668da901c55e891cec4c8749687359818
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887929"
---
# <a name="ksetup-domain"></a>ksetup domain

設定所有 Kerberos 作業的功能變數名稱。

## <a name="syntax"></a>語法

```
ksetup /domain <domainname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<domainname>` | 您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 contoso.com 或 contoso。|

### <a name="examples"></a>範例

若要使用子命令建立與有效網域（例如 Microsoft）的連接，請 `ksetup /mapuser` 輸入：

```
ksetup /mapuser principal@realm domain-user /domain domain-name
```

成功連線之後，您會收到新的 TGT，或將重新整理現有的 TGT。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup mapuser 命令](ksetup-mapuser.md)