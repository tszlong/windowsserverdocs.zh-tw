---
title: bitsadmin setclientcertificatebyname
description: 適用于**bitsadmin setclientcertificatebyname**的 Windows 命令主題，它會指定要在 HTTPS （SSL）要求中用於用戶端驗證的用戶端憑證主體名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2308bb5331f1555965b278a64bb7ab95e03779b
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123050"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname

指定要在 HTTPS （SSL）要求中用於用戶端驗證之用戶端憑證的主體名稱。

## <a name="syntax"></a>語法

```
bitsadmin /setclientcertificatebyname <job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| 工作 | 作業的顯示名稱或 GUID。 |
| store_location | 識別用來查閱憑證的系統存放區位置。 可能的值包括：<ul><li>1（CURRENT_USER）</li><li>2（LOCAL_MACHINE）</li><li>3（CURRENT_SERVICE）</li><li>4（服務）</li><li>5（使用者）</li><li>6（CURRENT_USER_GROUP_POLICY）</li><li>7（LOCAL_MACHINE_GROUP_POLICY）</li><li>8（LOCAL_MACHINE_ENTERPRISE）</li></ul> |
| store_name | 憑證存放區的名稱。 可能的值包括：<ul><li>CA （憑證授權單位單位憑證）</li><li>我的（個人憑證）</li><li>根（根憑證）</li><li>SPC （軟體發行者憑證）</li></ul> |
| subject_name | 憑證的名稱。 |

## <a name="examples"></a>範例

下列範例會指定用戶端憑證*我憑證*的名稱，以用於名為*myDownloadJob*之作業的 HTTPS （SSL）要求中的用戶端驗證。

```
C:\>bitsadmin /setclientcertificatebyname myDownloadJob 1 MY myCertificate
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)