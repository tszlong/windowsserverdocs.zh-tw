---
title: bitsadmin setclientcertificatebyid
description: Bitsadmin setclientcertificatebyid 命令的參考文章，此命令會指定用戶端憑證的識別碼，以供 HTTPS (SSL) 要求中的用戶端驗證使用
ms.topic: reference
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: aa49c0fc93972ce447c114ec20722afe67ca5dda
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631033"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid

指定用戶端憑證的識別碼，以用於 HTTPS (SSL) 要求中的用戶端驗證。

## <a name="syntax"></a>語法

```
bitsadmin /setclientcertificatebyid <job> <store_location> <store_name> <hexadecimal_cert_id>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| store_location | 識別用來查閱憑證的系統存放區位置，包括：<ul><li>CURRENT_USER</li><li>LOCAL_MACHINE</li><li>CURRENT_SERVICE</li><li>服務</li><li>使用者</li><li>CURRENT_USER_GROUP_POLICY</li><li>LOCAL_MACHINE_GROUP_POLICY</li><li>LOCAL_MACHINE_ENTERPRISE。</li></ul> |
| store_name | 憑證存放區的名稱，包括：<ul><li>CA (憑證授權單位單位憑證) </li><li>我的 (個人憑證) </li><li>根 (根憑證) </li><li>SPC (軟體發行者憑證) 。</li></ul> |
| hexadecimal_cert_id | 十六進位數位，代表憑證的雜湊。 |

## <a name="examples"></a>範例

若要在 HTTPS 中指定用戶端憑證的識別碼以用於用戶端驗證，請在名為 *myDownloadJob*的作業 (SSL) 要求：

```
bitsadmin /setclientcertificatebyid myDownloadJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
