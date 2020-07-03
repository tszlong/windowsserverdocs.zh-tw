---
title: auditpol remove
description: Auditpol remove 命令的參考文章，其會移除指定帳戶或所有帳戶的每個使用者稽核原則。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aada45bdc128c3122f459813d6f015f58532de18
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923721"
---
# <a name="auditpol-remove"></a>auditpol remove

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

移除指定帳戶或所有帳戶的每個使用者稽核原則。

若要對*每個使用者*原則執行*移除*作業，您必須擁有安全描述項中該物件集的**寫入**或**完整控制**許可權。 如果您擁有 [**管理審核和安全性記錄檔**] （SeSecurityPrivilege）使用者權限，也可以執行*移除*作業。 不過，此許可權允許執行整體*移除*作業所需的其他存取權。

## <a name="syntax"></a>語法

```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ------- | -------- |
| /user | 針對要刪除其每個使用者稽核原則的使用者，指定其安全識別碼（SID）或使用者名稱。 |
| /allusers | 移除所有使用者的每個使用者稽核原則。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要依名稱移除使用者 mikedan 的每個使用者稽核原則，請輸入：

```
auditpol /remove /user:mikedan
```

若要將使用者 mikedan 的每個使用者稽核原則移除 SID，請輸入：

```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```

若要移除所有使用者的每個使用者稽核原則，請輸入：

```
auditpol /remove /allusers
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
