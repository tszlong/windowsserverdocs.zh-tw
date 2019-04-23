---
title: ksetup:delenctypeattr
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c2cc96e8156cafd3846422596abe62513e275b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838129"
---
# <a name="ksetupdelenctypeattr"></a>ksetup:delenctypeattr



移除網域的加密類型屬性。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delenctypeattr <DomainName> 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName>|您要建立連線的網域名稱。 使用完整的網域名稱或名稱，例如 corp.contoso.com 或 contoso 的一個簡單的表單。|

## <a name="remarks"></a>備註

若要檢視 Kerberos 票證授權票證 (TGT) 和工作階段金鑰的加密類型，請執行**klist**命令，並檢視輸出。

成功或失敗的完成時，會顯示狀態訊息。

若要設定您想要連線至並使用的網域，請執行**ksetup /domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>範例

判斷此電腦上的目前加密類型：
```
klist
```
將網域設定 mit.contoso.com:
```
ksetup /domain mit.contoso.com
```
確認加密類型屬性是網域：
```
ksetup /getenctypeattr mit.contoso.com
```
移除網域 mit.contoso.com 組加密類型屬性：
```
ksetup /delenctypeattr mit.contoso.com
```

#### <a name="additional-references"></a>其他參考資料

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   [命令列語法關鍵](command-line-syntax-key.md)