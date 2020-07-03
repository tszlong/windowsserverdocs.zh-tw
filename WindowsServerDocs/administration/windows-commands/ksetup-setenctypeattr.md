---
title: ksetup setenctypeattr
description: Ksetup setenctypeattr 命令的參考文章，其會設定網域的加密類型屬性。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8bb8411a795d0167af1fc921fdf1c19febcb8527
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933908"
---
# <a name="ksetup-setenctypeattr"></a>ksetup setenctypeattr

設定網域的加密類型屬性。 成功或失敗完成時，會顯示狀態訊息。

您可以藉由執行**klist**命令並查看輸出，來查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型。 您可以藉由執行命令來設定要連接及使用的網域 `ksetup /domain <domainname>` 。

## <a name="syntax"></a>語法

```
ksetup /setenctypeattr <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<domainname>` | 您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 corp.contoso.com 或 contoso。 |
| 加密類型 | 必須是下列其中一種支援的加密類型：<ul><li>DES-CBC-CRC</li><li>DES-CBC-MD5</li><li>RC4-HMAC-MD5</li><li>AES128-CTS-HMAC-SHA1-96</li><li>AES256-CTS-HMAC-SHA1-96</li></ul> |

#### <a name="remarks"></a>備註

- 您可以藉由將命令中的加密類型與空格分隔，來設定或新增多個加密類型。 不過，您一次只能針對一個網域執行此動作。

### <a name="examples"></a>範例

若要查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型，請輸入：

```
klist
```

若要將網域設定為 corp.contoso.com，請輸入：

```
ksetup /domain corp.contoso.com
```

若要將加密類型屬性設定為 corp.contoso.com 網域的 AES-256-CTS-HMAC-96，請輸入：

```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```

若要確認加密類型屬性已設定為適用于網域，請輸入：

```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [klist 命令](klist.md)

- [ksetup 命令](ksetup.md)

- [ksetup 網域命令](ksetup-domain.md)

- [ksetup addenctypeattr 命令](ksetup-addenctypeattr.md)

- [ksetup getenctypeattr 命令](ksetup-getenctypeattr.md)

- [ksetup delenctypeattr 命令](ksetup-delenctypeattr.md)
