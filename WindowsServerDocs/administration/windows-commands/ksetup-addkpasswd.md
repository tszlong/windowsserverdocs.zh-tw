---
title: ksetup:addkpasswd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a85eb6dfe30c33126504744a7659fe2cc573087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856819"
---
# <a name="ksetupaddkpasswd"></a>ksetup:addkpasswd



新增領域的 Kerberos 密碼 (Kpasswd) 伺服器位址。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>參數

如果 Kerberos 領域，工作站會向支援 Kerberos 變更密碼通訊協定，您可以設定為使用 Kerberos 密碼伺服器執行 Windows 作業系統的用戶端電腦。 這項設定的領域的一端。

|參數|描述|
|---------|-----------|
|\<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM，且預設值會列為領域 = 當**ksetup**執行。|
|\<KpasswdName>|KDC 名稱所要做為 Kerberos 密碼伺服器是做為不區分大小寫的完整的網域名稱，例如 mitkdc.microsoft.com 所述。 如果省略 KDC 名稱，則 DNS 可能會用來尋找 Kdc 中。|

## <a name="remarks"></a>備註

如果 Kerberos 領域，工作站會向支援 Kerberos 變更密碼通訊協定，您可以設定為使用 Kerberos 密碼伺服器執行 Windows 作業系統的用戶端電腦。

執行命令**ksetup**確認 KDC 名稱。 如果**kpasswd =** 不會出現在輸出中，對應尚未設定。

您可以新增額外的 KDC 名稱一次。

## <a name="BKMK_Examples"></a>範例

設定領域 CORP.CONTOSO.COM，因此它會使用非 Windows KDC 伺服器上，mitkdc.contoso.com，做為密碼伺服器：
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
這會導致控制項進行驗證，它和領域之間的所有密碼的非 Windows Kerberos 密碼伺服器。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [命令列語法關鍵](command-line-syntax-key.md)