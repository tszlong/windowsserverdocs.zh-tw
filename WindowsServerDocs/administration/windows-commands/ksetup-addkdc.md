---
title: ksetup:addkdc
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3e0d38bdec11618561ee4acaa32ffdd06695fab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868529"
---
# <a name="ksetupaddkdc"></a>ksetup:addkdc



將指定的 Kerberos 領域的金鑰發佈中心 (KDC) 位址。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM，並列為預設領域時**ksetup**執行。 它是您嘗試要加入其他 KDC 這個領域。|
|\<KDCName>|KDC 名稱會指定為不區分大小寫的完整的網域名稱，例如 mitkdc.microsoft.com。 如果省略 KDC 名稱，DNS 會找出 Kdc。|

## <a name="remarks"></a>備註

這些對應會儲存在登錄中**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**。 若要部署到多部電腦的 Kerberos 領域組態資料，使用而不使用安全性設定的範本 嵌入式管理單元和原則散佈**ksetup**明確地在個別電腦上。

將會使用新的領域設定之前，必須重新啟動電腦。

若要確認電腦的預設領域名稱，或確定此命令運作如預期般，執行**ksetup**在命令提示字元並新增 kdc 驗證輸出。

## <a name="BKMK_Examples"></a>範例

設定非 Windows KDC 伺服器和工作站應該使用的領域：
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
在上述命令將本機電腦帳戶密碼設定為如同在同一部電腦的命令列執行 Ksetup 工具 」p@sswrd1%？。 然後重新啟動電腦。
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [命令列語法關鍵](command-line-syntax-key.md)