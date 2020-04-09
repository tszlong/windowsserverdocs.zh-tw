---
title: bitsadmin setclientcertificatebyname
description: 適用于 bitsadmin setclientcertificatebyname 的 Windows 命令主題，它會指定要在 HTTPS （SSL）要求中用於用戶端驗證的用戶端憑證主體名稱。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08ec6fd8c941234de36f14cd71ffa51c3b428acb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849651"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname

指定要在 HTTPS （SSL）要求中用於用戶端驗證之用戶端憑證的主體名稱。

## <a name="syntax"></a>語法

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Store_location|識別用來查閱憑證的系統存放區位置。 可能的值包括：</br>1（CURRENT_USER）</br>2（LOCAL_MACHINE）</br>3（CURRENT_SERVICE）</br>4（服務）</br>5（使用者）</br>6（CURRENT_USER_GROUP_POLICY）</br>7（LOCAL_MACHINE_GROUP_POLICY）</br>8（LOCAL_MACHINE_ENTERPRISE）|
|Store_name|憑證存放區的名稱。 可能的值包括：</br>CA （憑證授權單位單位憑證）</br>我的（個人憑證）</br>根（根憑證）</br>SPC （軟體發行者憑證）|
|Subject_name|憑證的名稱|

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會指定用戶端憑證*我憑證*的名稱，以用於名為*myJob*之作業的 HTTPS （SSL）要求中的用戶端驗證。
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)