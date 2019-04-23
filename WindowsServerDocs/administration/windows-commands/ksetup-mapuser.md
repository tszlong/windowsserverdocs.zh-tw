---
title: ksetup:mapuser
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2828f92b20cafcb571c81c8ceae28c741fbe025a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872859"
---
# <a name="ksetupmapuser"></a>ksetup:mapuser



對應至帳戶的 Kerberos 主體名稱。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /mapuser <Principal> <Account>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<主體 >|完整的網域任何的名稱主體;比方說， mike@corp.CONTOSO.COM。|
|\<Account>|任何帳戶或安全性群組名稱存在於此電腦上，例如客體、 網域使用者或系統管理員。|

## <a name="remarks"></a>備註

帳戶可以在明確地辨識，例如網域來賓。 或者，您可以使用萬用字元 （*） 以包含所有的帳戶。

如果省略帳戶名稱，則對應中刪除指定的主體。

如果它們存在有效的 Kerberos 票證，電腦將只會驗證指定領域的主體。

使用**ksetup**不含任何參數或引數，以查看目前的對應設定和預設領域。

每當對外部的金鑰發佈中心 (KDC) 和領域設定進行變更，需要重新啟動已在變更設定的電腦。

## <a name="BKMK_Examples"></a>範例

Kerberos 領域 CONTOSO 內的 Mike Danseglio 帳戶對應到此電腦上，授與他不必向這台電腦的所有成員的內建的 Guest 帳戶的權限的來賓帳戶：
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
移除 Mike Danseglio 帳戶以防止這台電腦。 其認證驗證從 CONTOSO 他這台電腦上的 guest 帳戶的對應：
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
將 CONTOSO Kerberos 領域內的 Mike Danseglio 帳戶對應至任何現有的帳戶，在此電腦上。 （如果只有標準使用者和來賓帳戶是在此電腦上作用，Mike 的權限會設定為那些）：
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
對應至任何現有的帳戶相同名稱的這台電腦上的 CONTOSO Kerberos 領域內的所有帳戶：
```
ksetup /mapuser * *
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)