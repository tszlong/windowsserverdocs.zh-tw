---
title: ksetup setrealm
description: '>ksetup setrealm 命令的參考文章，此命令會設定 Kerberos 領域的名稱。'
ms.topic: reference
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a06c5fef1127236319b580fbd6810269c58d1377
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627129"
---
# <a name="ksetup-setrealm"></a>ksetup setrealm

設定 Kerberos 領域的名稱。

> [!IMPORTANT]
> 不支援在網域控制站上設定 Kerberos 領域。 嘗試這麼做會導致警告和命令失敗。

## <a name="syntax"></a>語法

```
ksetup /setrealm <DNSdomainname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<DNSdomainname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 您可以使用完整功能變數名稱或簡單的名稱格式。 如果您未針對 DNS 名稱使用大寫，系統會要求您進行驗證以繼續。 |

### <a name="examples"></a>範例

若要將這部電腦的領域設定為特定的功能變數名稱，並將非網域控制站的存取許可權制為 CONTOSO Kerberos 領域，請輸入：

```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [ksetup removerealm](ksetup-removerealm.md)
