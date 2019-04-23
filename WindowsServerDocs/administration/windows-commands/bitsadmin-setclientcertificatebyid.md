---
title: bitsadmin setclientcertificatebyid
description: 適用於 Windows 命令主題**bitsadmin setclientcertificatebyid**指定要用於 HTTPS (SSL) 要求中的用戶端驗證的用戶端憑證的識別碼
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2424de18ee8aaec73b086207e8ef56d85df862fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863929"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid



指定要用於 HTTPS (SSL) 要求中的用戶端驗證的用戶端憑證的識別碼。

## <a name="syntax"></a>語法

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|Job|作業的顯示名稱或 GUID|
|Store_location|識別要用來查詢憑證的系統存放區的位置。 可能的值包括：</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 （使用者）</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|憑證存放區的名稱。 可能的值包括：</br>CA （憑證授權單位憑證）</br>MY （Personal 憑證）</br>根 （根憑證）</br>SPC （軟體發行者憑證）|
|Hexadecimal_cert_id|代表憑證的雜湊的十六進位數字|

## <a name="BKMK_examples"></a>範例

下列範例會指定要用於名為作業的 HTTPS (SSL) 要求中的用戶端驗證的用戶端憑證的識別碼*myJob*。
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)