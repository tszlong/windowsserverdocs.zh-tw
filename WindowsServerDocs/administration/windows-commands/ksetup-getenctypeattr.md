---
title: ksetup： getenctypeattr
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1ff55cfd204f76b42c5f1342b3cf206ee4c14f14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374991"
---
# <a name="ksetupgetenctypeattr"></a>ksetup： getenctypeattr



抓取網域的加密類型屬性。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /getenctypeattr <DomainName> 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName >|您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 corp.contoso.com 或 contoso。|

## <a name="remarks"></a>備註

若要查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型，請執行**klist**命令並查看輸出。

如果命令成功或失敗，則會在成功或失敗完成時顯示狀態訊息。

若要設定您想要連接並使用的網域，請執行**ksetup/domain \<DomainName >** 命令。

## <a name="BKMK_Examples"></a>典型

確認網域的 [加密類型] 屬性：
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