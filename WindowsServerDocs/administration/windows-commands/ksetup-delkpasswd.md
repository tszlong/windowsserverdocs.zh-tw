---
title: ksetup:delkpasswd
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e487389e0f27f58aacaea2b81d573dc9a965e42f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889079"
---
# <a name="ksetupdelkpasswd"></a>ksetup:delkpasswd

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

移除領域的 Kerberos 密碼伺服器 (Kpasswd)。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。
## <a name="syntax"></a>語法
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM，且預設值會列為領域 = 當**ksetup**執行。|
|<KpasswdName>|KDC 名稱以做為 Kerberos 密碼伺服器是做為不區分大小寫、 完整網域名稱，例如 mitkdc.contoso.com 所述。 如果省略 KDC 名稱，則 DNS 可能會用來尋找 Kdc 中。|
## <a name="remarks"></a>備註
執行命令**ksetup**確認 KDC 名稱。 如果**kpasswd =** 不會出現在輸出中，則尚未設定對應。 也將會列出多個對應，如果設定。
## <a name="BKMK_Examples"></a>範例
驗證領域 CORP.CONTOSO.COM 使用非 Windows KDC 伺服器 mitkdc.contoso.com 做為密碼伺服器：
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
若要確認命令成功，如預期般，執行**ksetup**確認領域 CORP.的 Windows 電腦上CONTOSO.COM 未對應到 （KDC 名稱） 的 Kerberos 密碼伺服器。
## <a name="additional-references"></a>其他參考資料
-   [ksetup](ksetup.md)
-   [ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [命令列語法關鍵](command-line-syntax-key.md)
