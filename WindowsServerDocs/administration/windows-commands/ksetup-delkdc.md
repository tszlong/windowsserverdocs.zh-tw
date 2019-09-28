---
title: ksetup： delkdc
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b918f8c2fa0ec09c2aae77517a0ee2c9e77ce2dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375144"
---
# <a name="ksetupdelkdc"></a>ksetup： delkdc



刪除 Kerberos 領域金鑰發佈中心（KDC）名稱的實例。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<RealmName >|領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM，在執行**ksetup**時，它會列為預設領域。 這是您嘗試刪除其他 KDC 的領域。|
|\<KDCName >|KDC 名稱會指定為不區分大小寫的完整功能變數名稱，例如 mitkdc.contoso.com。|

## <a name="remarks"></a>備註

這些對應會儲存在**HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**的登錄中。 若要從多部電腦移除領域設定資料，請使用 [安全性設定範本] 嵌入式管理單元和原則發佈，而不是在個別電腦上明確使用**ksetup** 。

在執行 Windows 2000 Server Service Pack 1 （SP1）和更早版本的電腦上，必須先重新開機電腦，才會使用變更的領域設定。

若要驗證電腦的預設領域名稱，或確認此命令是否如預期運作，請在命令提示字元中執行**ksetup** ，並確認已移除的 KDC 不存在於清單中。

## <a name="BKMK_Examples"></a>典型

這部電腦的安全性需求已變更，因此必須移除 Windows 領域和非 Windows 領域之間的連結。 首先，判斷要移除哪一個關聯，並產生現有關聯的輸出：
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