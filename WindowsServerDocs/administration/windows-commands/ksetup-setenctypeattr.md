---
title: ksetup:setenctypeattr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88fb913e-6b57-48d9-8c16-a035ab2977ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a91539ec7a9e0ce4c75d5165da1b88ae36d3fe6c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879199"
---
# <a name="ksetupsetenctypeattr"></a>ksetup:setenctypeattr



設定網域的加密類型屬性。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /setenctypeattr <Domain name> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName>|您要建立連線的網域名稱。 使用完整的網域名稱或名稱，例如 corp.contoso.com 或 contoso 的一個簡單的表單。|
|加密類型|必須是下列一種支援的加密類型：</br>-   DES-CBC-CRC</br>-   DES-CBC-MD5</br>-   RC4-HMAC-MD5</br>-   AES128-CTS-HMAC-SHA1-96</br>-   AES256-CTS-HMAC-SHA1-96|

## <a name="remarks"></a>備註

若要檢視 Kerberos 票證授權票證 (TGT) 和工作階段金鑰的加密類型，請執行**klist**命令，並檢視輸出。

您可以設定，或藉由以空格分隔的加密類型，在命令中將多個的加密類型。 不過，您才能進行一個網域一次。

如果命令成功或失敗，則會顯示狀態訊息。

若要設定您想要連線至並使用的網域，請執行**ksetup /domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>範例

判斷此電腦上的目前加密類型：
```
klist
```
設 corp.contoso.com 網域：
```
ksetup /domain corp.contoso.com
```
加密類型屬性設為 corp.contoso.com 網域 AES-256-CTS-HMAC-SHA1-96:
```
ksetup /setenctypeattr corp.contoso.com AES-256-CTS-HMAC-SHA1-96
```
確認已設定的加密類型的屬性，如預期般網域：
```
ksetup /getenctypeattr corp.contoso.com
```

#### <a name="additional-references"></a>其他參考資料

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:getenctypeattr](ksetup-getenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [命令列語法關鍵](command-line-syntax-key.md)