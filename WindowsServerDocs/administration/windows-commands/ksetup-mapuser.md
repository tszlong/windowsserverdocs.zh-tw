---
title: ksetup： mapuser
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f61c67fa21eccb77601b78aed51791259d609c5e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841481"
---
# <a name="ksetupmapuser"></a>ksetup： mapuser



將 Kerberos 主體的名稱對應至帳戶。 如需如何使用此命令的範例，請參閱[範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
ksetup /mapuser <Principal> <Account>
```

#### <a name="parameters"></a>參數

|  參數   |                                                   描述                                                   |
|--------------|-----------------------------------------------------------------------------------------------------------------|
| \<主體 > |              任何主體的完整功能變數名稱;例如，mike@corp.CONTOSO.COM。              |
|  \<帳戶 >  | 存在於此電腦上的任何帳戶或安全性群組名稱，例如 [來賓]、[網域使用者] 或 [系統管理員]。 |

## <a name="remarks"></a>備註

可以明確識別帳戶，例如網域來賓。 或者，您可以使用萬用字元（*）來包含所有帳戶。

如果省略帳戶名稱，則會刪除指定主體的對應。

只有在指定領域的主體出示有效的 Kerberos 票證時，電腦才會進行驗證。

請使用不含任何參數或引數的**ksetup**來查看目前的對應設定和預設領域。

每當對外部金鑰發佈中心（KDC）和領域設定進行變更時，就需要重新開機已變更設定的電腦。

## <a name="examples"></a><a name=BKMK_Examples></a>典型

將 Kerberos 領域 CONTOSO 內的 Mike Danseglio 帳戶對應到這部電腦上的 guest 帳戶，授與他內建來賓帳戶成員的擁有權限，而不需向此電腦驗證：
```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```
移除 Mike Danseglio 帳戶與這部電腦上來賓帳戶的對應，以防止他使用 CONTOSO 的認證向這部電腦進行驗證：
```
ksetup /mapuser mike@corp.CONTOSO.COM 
```
將 CONTOSO Kerberos 領域內的 Mike Danseglio 帳戶對應到這部電腦上任何現有的帳戶。 （如果只有標準使用者和來賓帳戶在這部電腦上為作用中，則 Mike 的許可權將會設定為那些）：
```
ksetup /mapuser mike@corp.CONTOSO.COM *
```
將 CONTOSO Kerberos 領域內的所有帳戶對應到此電腦上相同名稱的任何現有帳戶：
```
ksetup /mapuser * *
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)