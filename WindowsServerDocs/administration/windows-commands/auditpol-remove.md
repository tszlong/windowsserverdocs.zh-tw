---
title: auditpol remove
description: Auditpol remove 命令的參考文章，此命令會移除指定帳戶或所有帳戶的每個使用者稽核原則。
ms.topic: reference
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eee48822a082c3e7f5aa37cbd09a24059c94624e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029026"
---
# <a name="auditpol-remove"></a>auditpol remove

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

針對指定的帳號或所有帳戶移除個別使用者稽核原則。

若要在*每個使用者*的原則上執行*移除*作業，您必須擁有安全描述項中該物件集的**寫入**或**完整控制**許可權。 如果您有 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege) 使用者權限，也可以執行*移除*作業。 不過，此許可權可讓您不需要執行整體 *移除* 作業的額外存取權。

## <a name="syntax"></a>語法

```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /user | 指定要刪除每一使用者稽核原則之使用者的安全識別碼 (SID) 或使用者名稱。 |
| /allusers | 移除所有使用者的每個使用者稽核原則。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要依名稱移除使用者 mikedan 的每個使用者稽核原則，請輸入：

```
auditpol /remove /user:mikedan
```

若要依 SID 移除使用者 mikedan 的每個使用者稽核原則，請輸入：

```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```

若要移除所有使用者的每一使用者稽核原則，請輸入：

```
auditpol /remove /allusers
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
