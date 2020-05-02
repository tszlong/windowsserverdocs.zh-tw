---
title: ksetup： setenctypeattr
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cb7380a5fc65734902c6eed0b4b941eda6f6f5a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724564"
---
# <a name="ksetupsetenctypeattr"></a>ksetup： setenctypeattr



設定網域的加密類型屬性。

## <a name="syntax"></a>語法

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName>|您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 corp.contoso.com 或 contoso。|
|加密類型|必須是下列其中一種支援的加密類型：</br>-DES-CBC-CRC</br>-DES-CBC-MD5</br>-RC4-HMAC-MD5</br>-AES128-CTS-HMAC-SHA1-96</br>-AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>備註

若要查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型，請執行**klist**命令並查看輸出。

您可以藉由將命令中的加密類型與空格分隔，來設定或新增多個加密類型。 不過，您一次只能針對一個網域執行此動作。

如果命令成功或失敗，則會顯示狀態訊息。

若要設定您想要連接並使用的網域，請執行**ksetup/Domain \<DomainName>** 命令。

## <a name="examples"></a>範例

判斷這部電腦上設定的目前加密類型：
```
klist
```
將網域設定為 corp.contoso.com：
```
ksetup /domain corp.contoso.com
```
針對網域 corp.contoso.com，將 [加密類型] 屬性設定為 [AES-256-
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
確認 [加密類型] 屬性已設定為 [適用于網域]：
```
ksetup /getenctypeattr corp.contoso.com
```

## <a name="additional-references"></a>其他參考

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)