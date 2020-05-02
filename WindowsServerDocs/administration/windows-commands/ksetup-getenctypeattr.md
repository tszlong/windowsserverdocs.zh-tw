---
title: ksetup： getenctypeattr
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c7ec002-355e-474d-bc27-27215049f1a8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8363113d4fbb310d98b40d852b36a00f20320e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724638"
---
# <a name="ksetupgetenctypeattr"></a>ksetup： getenctypeattr



抓取網域的加密類型屬性。

## <a name="syntax"></a>語法

```
ksetup /getenctypeattr <DomainName> 
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName>|您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 corp.contoso.com 或 contoso。|

## <a name="remarks"></a>備註

若要查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型，請執行**klist**命令並查看輸出。

如果命令成功或失敗，則會在成功或失敗完成時顯示狀態訊息。

若要設定您想要連接並使用的網域，請執行**ksetup/Domain \<DomainName>** 命令。

## <a name="examples"></a>範例

確認網域的 [加密類型] 屬性：
```
ksetup /getenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>其他參考

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)