---
title: auditpol 清單
description: Auditpol list 命令的參考主題，其中會列出稽核原則類別和子類別，或列出已定義每個使用者稽核原則的使用者。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96ee4388c716c066a2e9b55b57dd2e70b4b4f69c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719101"
---
# <a name="auditpol-list"></a>auditpol 清單

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

列出稽核原則分類和子類別，或列出已定義每個使用者稽核原則的使用者。

若要對*每個使用者*原則執行*清單*作業，您必須擁有安全描述項中該物件集的 [**讀取**] 許可權。 如果您有 [**管理審核和安全性記錄檔**] （SeSecurityPrivilege）使用者權限，也可以執行*清單*作業。 不過，此許可權允許執行整體*清單*作業所需的其他存取權。

## <a name="syntax"></a>語法

```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ------- | -------- |
| /user | 抓取已定義每個使用者稽核原則的所有使用者。 如果與/v 參數搭配使用，則也會顯示使用者的安全識別碼（SID）。 |
| /類別 | 顯示系統所瞭解的類別名稱。 如果與/v 參數搭配使用，則也會顯示類別全域唯一識別碼（GUID）。 |
| /subcategory | 顯示子類別目錄的名稱及其相關聯的 GUID。 |
| /v | 顯示具有分類或子類別的 GUID，或與/user 搭配使用時，會顯示每個使用者的 SID。 |
| /r | 以逗號分隔值（CSV）格式，將輸出顯示為報告。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要列出所有具有已定義稽核原則的使用者，請輸入：

```
auditpol /list /user
```

若要列出擁有已定義稽核原則及其關聯 SID 的所有使用者，請輸入：

```
auditpol /list /user /v
```

若要以報表格式列出所有分類和子類別目錄，請輸入：

```
auditpol /list /subcategory:* /r
```

若要列出 [詳細追蹤] 和 [DS 存取] 分類的子類別，請輸入：

```
auditpol /list /subcategory:detailed Tracking,DS Access
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
