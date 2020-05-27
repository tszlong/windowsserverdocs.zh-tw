---
title: ksetup mapuser
description: Ksetup mapuser 命令的參考主題，其會將 Kerberos 主體的名稱對應至帳戶。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ac2f3e30b3057ceea4376d7ffe8286875d5301d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817668"
---
# <a name="ksetup-mapuser"></a>ksetup mapuser

將 Kerberos 主體的名稱對應至帳戶。

## <a name="syntax"></a>語法

```
ksetup /mapuser <principal> <account>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<principal>` | 指定任何主體使用者的完整功能變數名稱。 例如： mike@corp.CONTOSO.COM 。 如果您未指定帳戶參數，則會刪除指定主體的對應。 |
| `<account>` | 指定存在於此電腦上的任何帳戶或安全性群組名稱，例如 [**來賓**]、[**網域使用者**] 或 [**系統管理員**]。 如果省略此參數，則會刪除指定主體的對應。 |

#### <a name="remarks"></a>備註

- 可以明確識別帳戶（例如**網域來賓**），或者您可以使用萬用字元（*）來包含所有帳戶。

- 只有在指定領域的主體出示有效的 Kerberos 票證時，電腦才會進行驗證。

- 每當對外部金鑰發佈中心（KDC）和領域設定進行變更時，就需要重新開機已變更設定的電腦。

### <a name="examples"></a>範例

若要查看目前的對應設定和預設領域，請輸入：

```
ksetup
```

若要將 Kerberos 領域 CONTOSO 內的 Mike Danseglio 帳戶對應到這部電腦上的 guest 帳戶，授與他內建來賓帳戶成員的擁有權限，而不需向此電腦驗證，請輸入：

```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```

若要將 Mike Danseglio 的帳戶對應到這部電腦上的來賓帳戶，以避免他使用 CONTOSO 的認證向這部電腦進行驗證，請輸入：

```
ksetup /mapuser mike@corp.CONTOSO.COM
```

若要將 CONTOSO Kerberos 領域內的 Mike Danseglio 帳戶對應到這部電腦上的任何現有帳戶，請輸入：

```
ksetup /mapuser mike@corp.CONTOSO.COM *
```

> [!NOTE]
> 如果只有標準使用者和來賓帳戶在這部電腦上為作用中，則 Mike 的許可權會設定為這些。

若要將 CONTOSO Kerberos 領域內的所有帳戶對應到這部電腦上同名的現有帳戶，請輸入：

```
ksetup /mapuser * *
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)
