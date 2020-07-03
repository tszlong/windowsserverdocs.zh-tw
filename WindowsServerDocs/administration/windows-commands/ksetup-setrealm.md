---
title: ksetup setrealm
description: Ksetup setrealm 命令的參考文章，其會設定 Kerberos 領域的名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c6fa5573322237dfee5909d9afc2e99696ac82b3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933128"
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

| 參數 | 說明 |
| --------- | ----------- |
| `<DNSdomainname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 您可以使用完整功能變數名稱或簡單格式的名稱。 如果您的 DNS 名稱未使用大寫，系統會要求您進行驗證以繼續進行。 |

### <a name="examples"></a>範例

若要將這部電腦的領域設定為特定的功能變數名稱，並將非網域控制站的存取許可權制為 CONTOSO Kerberos 領域，請輸入：

```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup removerealm](ksetup-removerealm.md)
