---
title: auditpol list
description: Auditpol list 命令的參考文章，其中列出稽核原則類別和子類別，或列出定義了每個使用者稽核原則的使用者。
ms.topic: reference
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b49b138de8f25f25b00e593d6bd02620a3f2f624
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029076"
---
# <a name="auditpol-list"></a>auditpol list

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出稽核原則類別和子類別，或列出針對其定義個別使用者稽核原則的使用者。

若要對*每個使用者*原則執行*清單*作業，您必須擁有安全描述項中該物件集的 [**讀取**] 許可權。 如果您有 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege) 使用者權限，也可以執行*清單*作業。 不過，此許可權可讓您不需要執行整體 *清單* 作業的額外存取權。

## <a name="syntax"></a>語法

```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /user | 抓取已定義每個使用者稽核原則的所有使用者。 如果搭配/v 參數使用，則也會顯示使用者的安全識別碼 (SID) 。 |
| /類別 | 顯示系統所瞭解的類別名稱。 如果搭配/v 參數使用，則也會顯示類別全域唯一識別碼 (GUID) 。 |
| /subcategory | 顯示子類別目錄及其相關聯 GUID 的名稱。 |
| /v | 顯示具有類別或子類別的 GUID，或搭配/user 使用時，會顯示每個使用者的 SID。 |
| /r | 以逗點分隔值 (CSV) 格式，將輸出顯示為報表。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要列出所有具有已定義稽核原則的使用者，請輸入：

```
auditpol /list /user
```

若要列出已定義稽核原則及其相關 SID 的所有使用者，請輸入：

```
auditpol /list /user /v
```

若要列出報表格式的所有類別目錄和子類別目錄，請輸入：

```
auditpol /list /subcategory:* /r
```

若要列出詳細追蹤和 DS 存取類別的子類別，請輸入：

```
auditpol /list /subcategory:detailed Tracking,DS Access
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
