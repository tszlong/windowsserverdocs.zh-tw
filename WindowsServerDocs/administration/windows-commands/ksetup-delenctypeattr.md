---
title: ksetup： delenctypeattr
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc25ef3-e271-4229-a712-72c507df55aa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c650b973ac34e28394d5b6ec38142a058ad76338
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841761"
---
# <a name="ksetupdelenctypeattr"></a>ksetup： delenctypeattr



移除網域的加密類型屬性。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /delenctypeattr <DomainName> 
```

#### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<DomainName >|您想要建立連接的網功能變數名稱稱。 使用完整功能變數名稱或簡單格式的名稱，例如 corp.contoso.com 或 contoso。|

## <a name="remarks"></a>備註

若要查看 Kerberos 票證授權票證（TGT）和工作階段金鑰的加密類型，請執行**klist**命令並查看輸出。

成功或失敗完成時，會顯示狀態訊息。

若要設定您想要連接並使用的網域，請執行**ksetup/domain \<DomainName >** 命令。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

判斷這部電腦上設定的目前加密類型：
```
klist
```
將網域設定為 mit.contoso.com：
```
ksetup /domain mit.contoso.com
```
確認網域的 [加密類型] 屬性為何：
```
ksetup /getenctypeattr mit.contoso.com
```
移除網域 mit.contoso.com 的 [設定加密類型] 屬性：
```
ksetup /delenctypeattr mit.contoso.com
```

## <a name="additional-references"></a>其他參考資料

-   [Klist](klist.md)
-   [Ksetup:domain](ksetup-domain.md)
-   [Ksetup:addenctypeattr](ksetup-addenctypeattr.md)
-   [Ksetup:setenctypeattr](ksetup-setenctypeattr.md)
-   [Ksetup:delenctypeattr](ksetup-delenctypeattr.md)
-   - [命令列語法關鍵](command-line-syntax-key.md)