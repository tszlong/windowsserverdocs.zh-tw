---
title: bitsadmin setclientcertificatebyid
description: 適用于**bitsadmin setclientcertificatebyid**的 Windows 命令主題，它會指定要在 HTTPS （SSL）要求中用於用戶端驗證的用戶端憑證識別碼
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 376bb850664a5ed569488634029cb7384856f158
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123040"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

指定要在 HTTPS （SSL）要求中用於用戶端驗證的用戶端憑證識別碼。

## <a name="syntax"></a>語法

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| store_location | 識別用來查閱憑證之系統存放區的位置，包括：<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>伺服器</li><li>使用者</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE。</li></ul> |
| store_name | 憑證存放區的名稱，包括：<ul><li>CA （憑證授權單位單位憑證）</li><li>我的（個人憑證）</li><li>根（根憑證）</li><li>SPC （軟體發行者憑證）。</li></ul> |
| hexadecimal_cert_id | 十六進位數位，代表憑證的雜湊。 |

## <a name="examples"></a>範例

下列範例會針對名為*myDownloadJob*的作業，在 HTTPS （SSL）要求中指定用戶端憑證的識別碼，以用於用戶端驗證。

```
C:\>bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)