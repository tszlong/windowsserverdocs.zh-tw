---
title: bitsadmin setclientcertificatebyname
description: 適用于**bitsadmin setclientcertificatebyname**的 Windows 命令主題-指定 HTTPS （SSL）要求中用於用戶端驗證的用戶端憑證主體名稱。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de2e84401673848ecc8823bb6dd3f91224d9a87e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380676"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname



指定要在 HTTPS （SSL）要求中用於用戶端驗證之用戶端憑證的主體名稱。

## <a name="syntax"></a>語法

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Store_location|識別用來查閱憑證的系統存放區位置。 可能的值包括：</br>1（CURRENT_USER）</br>2（LOCAL_MACHINE）</br>3（CURRENT_SERVICE）</br>4（服務）</br>5（使用者）</br>6（CURRENT_USER_GROUP_POLICY）</br>7（LOCAL_MACHINE_GROUP_POLICY）</br>8（LOCAL_MACHINE_ENTERPRISE）|
|Store_name|憑證存放區的名稱。 可能的值包括：</br>CA （憑證授權單位單位憑證）</br>我的（個人憑證）</br>根（根憑證）</br>SPC （軟體發行者憑證）|
|Subject_name|憑證的名稱|

## <a name="BKMK_examples"></a>典型

下列範例會指定用戶端憑證*我憑證*的名稱，以用於名為*myJob*之作業的 HTTPS （SSL）要求中的用戶端驗證。
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)