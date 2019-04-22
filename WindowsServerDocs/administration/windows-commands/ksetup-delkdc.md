---
title: ksetup:delkdc
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e2fa065ca60338b04cc8718e199805cc494c908
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814559"
---
# <a name="ksetupdelkdc"></a>ksetup:delkdc



刪除執行個體的金鑰發佈中心 (KDC) 的 Kerberos 領域的名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName>|領域名稱會指定為大寫的 DNS 名稱，例如 CORP.CONTOSO.COM，並列為預設領域時**ksetup**執行。 它是您嘗試刪除其他 KDC 這個領域。|
|\<KDCName>|KDC 名稱會指定為不區分大小寫、 完整網域名稱，例如 mitkdc.contoso.com。|

## <a name="remarks"></a>備註

這些對應會儲存在登錄**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**。 若要移除領域組態資料從多部電腦，使用而不使用安全性設定的範本 嵌入式管理單元和原則散佈**ksetup**明確地在個別電腦上。

在執行 Windows 2000 Server Service Pack 1 (SP1) 或更早版本的電腦，電腦必須重新啟動之前將會使用已變更的領域設定組態。

若要確認電腦的預設領域名稱，或確定此命令運作如預期般，執行**ksetup**在命令提示字元，並確認已移除的 KDC 不存在於清單中。

## <a name="BKMK_Examples"></a>範例

此電腦的安全性需求有變更，因此必須移除 Windows 領域與非 Windows 領域之間的連結。 首先，判斷要移除，並產生輸出的現有關聯性的關聯：
```
ksetup
```
使用下列命令來移除關聯：
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>其他參考資料

-   [Ksetup](ksetup.md)
-   [命令列語法關鍵](command-line-syntax-key.md)