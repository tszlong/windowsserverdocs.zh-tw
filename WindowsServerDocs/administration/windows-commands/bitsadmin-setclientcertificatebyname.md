---
title: bitsadmin setclientcertificatebyname
description: Bitsadmin setclientcertificatebyname 命令的參考文章，它會指定用戶端憑證的主體名稱，以在 HTTPS (SSL) 要求中用於用戶端驗證。
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d51924153af90991c9417307d1f57e5d745dfa3
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893256"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname

在 HTTPS (SSL) 要求中指定用戶端憑證的主體名稱，以用於用戶端驗證。

## <a name="syntax"></a>語法

```
bitsadmin /setclientcertificatebyname <job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 作業 | 作業的顯示名稱或 GUID。 |
| store_location | 識別用來查閱憑證的系統存放區位置。 可能的值包括：<ul><li>1 (CURRENT_USER) </li><li>2 (LOCAL_MACHINE) </li><li>3 (CURRENT_SERVICE) </li><li>4 (服務) </li><li>5 (使用者) </li><li>6 (CURRENT_USER_GROUP_POLICY) </li><li>7 (LOCAL_MACHINE_GROUP_POLICY) </li><li>8 (LOCAL_MACHINE_ENTERPRISE) </li></ul> |
| store_name | 憑證存放區的名稱。 可能的值包括：<ul><li>CA (憑證授權單位單位憑證) </li><li>我的 (個人憑證) </li><li>根 (根憑證) </li><li>SPC (軟體發行者憑證) </li></ul> |
| subject_name | 憑證的名稱。 |

## <a name="examples"></a>範例

若要在 HTTPS 中指定要用於用戶端驗證的用戶端憑證*我憑證*名稱 (SSL) 名為*myDownloadJob*的作業要求：

```
bitsadmin /setclientcertificatebyname myDownloadJob 1 MY myCertificate
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin 命令](bitsadmin.md)
