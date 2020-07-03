---
title: bitsadmin setclientcertificatebyid
description: Bitsadmin setclientcertificatebyid 命令的參考文章，它會指定要在 HTTPS （SSL）要求中用於用戶端驗證的用戶端憑證識別碼
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfa3a65051cad9ef21f146888fc9d0e3e80f4023
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927889"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

指定要在 HTTPS （SSL）要求中用於用戶端驗證的用戶端憑證識別碼。

## <a name="syntax"></a>語法

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| store_location | 識別用來查閱憑證之系統存放區的位置，包括：<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>伺服器</li><li>使用者</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE。</li></ul> |
| store_name | 憑證存放區的名稱，包括：<ul><li>CA （憑證授權單位單位憑證）</li><li>我的（個人憑證）</li><li>根（根憑證）</li><li>SPC （軟體發行者憑證）。</li></ul> |
| hexadecimal_cert_id | 十六進位數位，代表憑證的雜湊。 |

## <a name="examples"></a>範例

若要指定用戶端憑證的識別碼，以用於名為*myDownloadJob*之作業的 HTTPS （SSL）要求中的用戶端驗證：

```
bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
