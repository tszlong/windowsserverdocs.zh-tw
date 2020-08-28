---
title: ksetup addkpasswd
description: '>ksetup addkpasswd 命令的參考文章，此命令會為領域新增 Kerberos 密碼 (kpasswd) 伺服器位址。'
ms.topic: reference
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62123fc8a04006078c42894ee53f11346dec0e59
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037946"
---
# <a name="ksetup-addkpasswd"></a>ksetup addkpasswd

將 Kerberos 密碼 (kpasswd) 伺服器位址新增至領域。

## <a name="syntax"></a>語法

```
ksetup /addkpasswd <realmname> [<kpasswdname>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，並在 **>ksetup**執行時列為預設領域或**領域 =** 。 |
| `<kpasswdname>` | 指定 Kerberos 密碼伺服器。 它會被視為不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 如果省略 KDC 名稱，則可能會使用 DNS 來尋找 Kdc。 |

#### <a name="remarks"></a>備註

- 如果工作站要驗證的 Kerberos 領域支援 Kerberos 變更密碼通訊協定，您可以將執行 Windows 作業系統的用戶端電腦設定為使用 Kerberos 密碼伺服器。

- 您可以一次新增一個其他 KDC 名稱。

### <a name="examples"></a>範例

設定 CORP。CONTOSO.COM 領域若要使用非 Windows KDC 伺服器 mitkdc.contoso.com 作為密碼伺服器，請輸入：

```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

若要確認已設定 KDC 名稱，請輸入， `ksetup` 然後再查看輸出，並尋找 [ **kpasswd =**] 文字。 如果您沒有看到文字，表示尚未設定對應。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup delkpasswd 命令](ksetup-delkpasswd.md)
