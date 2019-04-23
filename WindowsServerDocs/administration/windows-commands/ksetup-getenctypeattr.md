---
title: ksetup:getenctypeattr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 302f94616f98eb350332b08ad37a58305a0a0be1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841989"
---
# <a name="ksetupgetenctypeattr"></a>ksetup:getenctypeattr



擷取網域的加密類型屬性。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /getenctypeattr <DomainName> 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName>|您要建立連線的網域名稱。 使用完整的網域名稱或名稱，例如 corp.contoso.com 或 contoso 的一個簡單的表單。|

## <a name="remarks"></a>備註

若要檢視 Kerberos 票證授權票證 (TGT) 和工作階段金鑰的加密類型，請執行**klist**命令，並檢視輸出。

如果命令成功或失敗，狀態訊息會顯示成功或失敗的完成時。

若要設定您想要連線至並使用的網域，請執行**ksetup /domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>範例

確認網域的加密類型屬性：
```
ksetup /getenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>其他參考資料

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [命令列語法關鍵](command-line-syntax-key.md)