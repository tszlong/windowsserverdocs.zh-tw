---
title: ksetup addkpasswd
description: Ksetup addkpasswd 命令的參考文章，它會新增領域的 Kerberos 密碼（kpasswd）伺服器位址。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2db728684e713df35a39995c8ca95196f0b745ed
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925558"
---
# <a name="ksetup-addkpasswd"></a>ksetup addkpasswd

新增領域的 Kerberos 密碼（kpasswd）伺服器位址。

## <a name="syntax"></a>語法

```
ksetup /addkpasswd <realmname> [<kpasswdname>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM，且在執行**ksetup**時列為預設領域或**領域 =** 。 |
| `<kpasswdname>` | 指定 Kerberos 密碼伺服器。 它會指定為不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。 如果省略 KDC 名稱，則可能會使用 DNS 來尋找 Kdc。 |

#### <a name="remarks"></a>備註

- 如果工作站要驗證的 Kerberos 領域支援 Kerberos 變更密碼通訊協定，您可以將執行 Windows 作業系統的用戶端電腦設定為使用 Kerberos 密碼伺服器。

- 您可以一次新增一個額外的 KDC 名稱。

### <a name="examples"></a>範例

以設定 CORP。CONTOSO.COM 領域若要使用非 Windows KDC 伺服器，mitkdc.contoso.com 作為密碼伺服器，請輸入：

```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```

若要確認已設定 KDC 名稱，請輸入， `ksetup` 然後再查看輸出，尋找文字**kpasswd =**。 如果您看不到文字，表示尚未設定對應。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup delkpasswd 命令](ksetup-delkpasswd.md)
