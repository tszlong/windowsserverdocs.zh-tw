---
title: bitsadmin setclientcertificatebyid
description: Bitsadmin setclientcertificatebyid 命令的參考文章，它會指定要在 HTTPS (SSL) 要求中用於用戶端驗證的用戶端憑證識別碼
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e46219a52eda48ddb3e203730e6275fb5e534020
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893261"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

指定要在 HTTPS (SSL) 要求中用於用戶端驗證的用戶端憑證識別碼。

## <a name="syntax"></a>語法

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| store_location | 識別用來查閱憑證之系統存放區的位置，包括：<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>伺服器</li><li>使用者</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE。</li></ul> |
| store_name | 憑證存放區的名稱，包括：<ul><li>CA (憑證授權單位單位憑證) </li><li>我的 (個人憑證) </li><li>根 (根憑證) </li><li>SPC (軟體發行者憑證) 。</li></ul> |
| hexadecimal_cert_id | 十六進位數位，代表憑證的雜湊。 |

## <a name="examples"></a>範例

若要在 HTTPS 中指定用戶端憑證的識別碼以用於用戶端驗證，請 (SSL) 名為*myDownloadJob*的作業要求：

```
bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
