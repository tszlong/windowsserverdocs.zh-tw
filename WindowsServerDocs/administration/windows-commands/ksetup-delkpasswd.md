---
title: ksetup delkpasswd
description: '>ksetup delkpasswd 命令的參考文章，此命令會移除領域的 Kerberos 密碼伺服器 (kpasswd) 。'
ms.topic: reference
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 52624252953770ebe131a7c31618058232a21d05
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639673"
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
| `<realmname>` |  指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，並在 **>ksetup**執行時列為預設領域或**領域 =** 。 |
| `<kpasswdname>` | 指定 Kerberos 密碼伺服器。 它會被視為不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 如果省略 KDC 名稱，則可能會使用 DNS 來尋找 Kdc。 |

### <a name="examples"></a>範例

確認領域 CORP。CONTOSO.COM 使用非 Windows KDC 伺服器 mitkdc.contoso.com 作為密碼伺服器，請輸入：

```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

確認領域 CORP。CONTOSO.COM 未對應到 Kerberos 密碼伺服器 (KDC 名稱) ，請 `ksetup` 在 Windows 電腦上輸入，然後查看輸出。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup delkpasswd 命令](ksetup-delkpasswd.md)
