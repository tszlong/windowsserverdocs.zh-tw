---
title: ksetup domain
description: '>ksetup 網域命令的參考文章，此命令會設定所有 Kerberos 作業的功能變數名稱。'
ms.topic: reference
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c9e89023e127318139672581cbab267a34c67a58
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025532"
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
| `<domainname>` | 您要建立連線的功能變數名稱。 使用完整功能變數名稱或簡單格式的名稱，例如 contoso.com 或 contoso。|

### <a name="examples"></a>範例

若要使用子命令建立與有效網域（例如 Microsoft）的連接，請 `ksetup /mapuser` 輸入：

```
ksetup /mapuser principal@realm domain-user /domain domain-name
```

成功連線之後，您將會收到新的 TGT，或將重新整理現有的 TGT。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup mapuser 命令](ksetup-mapuser.md)