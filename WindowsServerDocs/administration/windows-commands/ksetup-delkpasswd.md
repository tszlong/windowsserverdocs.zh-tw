---
title: ksetup delkpasswd
description: Ksetup delkpasswd 命令的參考文章，它會移除領域 (kpasswd) 的 Kerberos 密碼伺服器。
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f507b259b77e8be15ade8d01f3666221e94f9192
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887964"
---
# <a name="ksetup-delkpasswd"></a>ksetup delkpasswd

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

移除領域 (kpasswd) 的 Kerberos 密碼伺服器。

## <a name="syntax"></a>語法

```
ksetup /delkpasswd <realmname> <kpasswdname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` |  指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，且在執行**ksetup**時列為預設領域或**領域 =** 。 |
| `<kpasswdname>` | 指定 Kerberos 密碼伺服器。 它會指定為不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 如果省略 KDC 名稱，則可能會使用 DNS 來尋找 Kdc。 |

### <a name="examples"></a>範例

確保領域 CORP。CONTOSO.COM 使用非 Windows KDC 伺服器 mitkdc.contoso.com 作為密碼伺服器，請輸入：

```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

確保領域 CORP。CONTOSO.COM 未對應至 (KDC 名稱) 的 Kerberos 密碼伺服器，請 `ksetup` 在 Windows 電腦上輸入，然後再查看輸出。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup delkpasswd 命令](ksetup-delkpasswd.md)
