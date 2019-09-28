---
title: ksetup： addkpasswd
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 72c27cb6b068dc46cd58e753b4b08d68b39bfb20
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375189"
---
# <a name="ksetupaddkpasswd"></a>ksetup： addkpasswd



新增領域的 Kerberos 密碼（Kpasswd）伺服器位址。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>參數

如果工作站要驗證的 Kerberos 領域支援 Kerberos 變更密碼通訊協定，您可以將執行 Windows 作業系統的用戶端電腦設定為使用 Kerberos 密碼伺服器。 此設定是在領域端設定。

|參數|描述|
|---------|-----------|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM，且在執行**ksetup**時列為預設領域或領域 =。|
|\<KpasswdName >|作為 Kerberos 密碼伺服器使用的 KDC 名稱會被視為不區分大小寫的完整功能變數名稱，例如 mitkdc.microsoft.com。 如果省略 KDC 名稱，則可能會使用 DNS 來尋找 Kdc。|

## <a name="remarks"></a>備註

如果工作站要驗證的 Kerberos 領域支援 Kerberos 變更密碼通訊協定，您可以將執行 Windows 作業系統的用戶端電腦設定為使用 Kerberos 密碼伺服器。

執行命令**ksetup**來驗證 KDC 名稱。 如果**kpasswd =** 未出現在輸出中，表示尚未設定對應。

您可以一次新增一個額外的 KDC 名稱。

## <a name="BKMK_Examples"></a>典型

設定領域 CORP。CONTOSO.COM，使其使用非 Windows KDC 伺服器 mitkdc.contoso.com，做為密碼伺服器：
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
這會導致非 Windows Kerberos 密碼伺服器，以控制它與領域之間驗證的所有密碼。

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [命令列語法關鍵](command-line-syntax-key.md)