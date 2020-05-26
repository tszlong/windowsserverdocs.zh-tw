---
title: ksetup 網域
description: Ksetup 網域命令的參考主題，其會設定所有 Kerberos 作業的功能變數名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ef766e3-6071-44f2-946b-22ea5b61a508
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1d497f2bc76bae8a95b077658c661e0fdc1e93f3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817798"
---
# <a name="ksetup-domain"></a>ksetup 網域

設定所有 Kerberos 作業的功能變數名稱。

## <a name="syntax"></a>語法

```
ksetup /domain <domainname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<domainname>` | 您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 contoso.com 或 contoso。|

### <a name="examples"></a>範例

若要使用子命令建立與有效網域（例如 Microsoft）的連接，請 `ksetup /mapuser` 輸入：

```
ksetup /mapuser principal@realm domain-user /domain domain-name
```

成功連線之後，您會收到新的 TGT，或將重新整理現有的 TGT。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup mapuser 命令](ksetup-mapuser.md)