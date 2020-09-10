---
title: ksetup mapuser
description: '>ksetup mapuser 命令的參考文章，此命令會將 Kerberos 主體的名稱對應至帳戶。'
ms.topic: reference
ms.assetid: 84113e6e-89ff-488a-9cd0-f14bbf23b543
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e8e676218455147e68f84b42bcbad2dcd7285a01
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622789"
---
# <a name="ksetup-mapuser"></a>ksetup mapuser

將 Kerberos 主體的名稱對應到帳戶。

## <a name="syntax"></a>語法

```
ksetup /mapuser <principal> <account>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<principal>` | 指定任何主體使用者的完整功能變數名稱。 例如： mike@corp.CONTOSO.COM 。 如果您未指定帳戶參數，則會刪除指定主體的對應。 |
| `<account>` | 指定存在於這部電腦上的任何帳戶或安全性群組名稱，例如 [ **來賓**]、[ **網域使用者**] 或 [ **系統管理員**]。 如果省略此參數，則會刪除指定主體的對應。 |

#### <a name="remarks"></a>備註

- 您可以明確識別帳戶（例如 **網域來賓**），也可以使用萬用字元 ( * ) 來包含所有帳戶。

- 電腦只會在提供有效的 Kerberos 票證時驗證指定領域的主體。

- 每當對外部金鑰發佈中心 (KDC) 和領域設定進行變更時，就需要重新開機已變更設定的電腦。

### <a name="examples"></a>範例

若要查看目前的對應設定和預設領域，請輸入：

```
ksetup
```

若要在 Kerberos 領域 CONTOSO 中將 Mike Danseglio 的帳戶對應到這部電腦上的 guest 帳戶，請將內建來賓帳戶成員的擁有權限授與該帳戶，而不需要向此電腦驗證，請輸入：

```
ksetup /mapuser mike@corp.CONTOSO.COM guest
```

若要移除 Mike Danseglio 的帳戶與這部電腦上的 guest 帳戶的對應，以防止他使用 CONTOSO 的認證向這部電腦進行驗證，請輸入：

```
ksetup /mapuser mike@corp.CONTOSO.COM
```

若要將 CONTOSO Kerberos 領域內的 Mike Danseglio 帳戶對應到這部電腦上任何現有的帳戶，請輸入：

```
ksetup /mapuser mike@corp.CONTOSO.COM *
```

> [!NOTE]
> 如果此電腦上只有標準使用者和來賓帳戶在使用中，則會將 Mike 的許可權設定為這些帳戶。

若要將 CONTOSO Kerberos 領域內的所有帳戶對應到此電腦上相同名稱的任何現有帳戶，請輸入：

```
ksetup /mapuser * *
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)
