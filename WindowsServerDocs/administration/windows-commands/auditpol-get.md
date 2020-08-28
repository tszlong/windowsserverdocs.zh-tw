---
title: auditpol get
description: Auditpol get 命令的參考文章，此命令會抓取系統原則、每個使用者的原則、審核選項和 audit 安全描述項物件。
ms.topic: reference
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23de44ebc9bc91ad4db52ee7362b14d9c93648d8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029086"
---
# <a name="auditpol-get"></a>auditpol get

> 適用于： Windows Server (半年通道) 、Windows Server、2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

抓取系統原則、每個使用者的原則、審核選項和 audit 安全描述項物件。

若要對*每個使用者*和*系統*策略執行*get*作業，您必須擁有安全描述項中該物件集的 [**讀取**] 許可權。 如果您有 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege) 使用者權限，也可以執行*get*作業。 不過，此許可權可讓您不需要執行整體 *get* 作業的額外存取權。

## <a name="syntax"></a>語法

```
auditpol /get
[/user[:<username>|<{sid}>]]
[/category:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/subcategory:*|<name>|<{guid}>[,:<name|<{guid}> ]]
[/option:<option name>]
[/sd]
[/r]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /user | 顯示查詢每位使用者稽核原則的安全性主體。 必須指定/category 或/subcategory 參數。 您可以將使用者指定為 (SID) 或名稱的安全識別碼。 如果未指定使用者帳戶，則會查詢系統稽核原則。 |
| /類別 | 全域唯一識別碼指定的一或多個審核分類 (GUID) 或名稱。 星號 ( * ) 可用來指出應該查詢所有的 audit 類別目錄。 |
| /subcategory | GUID 或名稱所指定的一或多個審核子類別。 |
| /sd | 抓取用來將存取權委派給稽核原則的安全描述項。 |
| /option | 抓取 CrashOnAuditFail、FullprivilegeAuditing、AuditBaseObjects 或 AuditBasedirectories 選項的現有原則。 |
| /r | 顯示報表格式的輸出、以逗號分隔的值 (CSV) 。 |
| /? | 在命令提示字元顯示說明。 |

### <a name="remarks"></a>備註

所有類別目錄和子類別都可以透過以引號括住的 GUID 或名稱 ( ") 來指定。 使用者可以透過 SID 或名稱來指定。

## <a name="examples"></a>範例

若要抓取 Guest 帳戶的每個使用者稽核原則，並顯示系統、詳細追蹤和物件存取類別的輸出，請輸入：

```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:System,detailed Tracking,Object Access
```

> [!NOTE]
> 在兩種情況下，此命令很有用。 1) 監視特定的使用者帳戶是否有可疑的活動時，您可以使用 `/get` 包含原則來啟用其他審核，以使用命令以取得特定類別的結果。 2) 如果帳戶的 audit 設定正在記錄許多但多餘的事件，您可以使用 `/get` 命令來篩選掉具有排除原則之帳戶的無關事件。 如需所有類別的清單，請使用 `auditpol /list /category` 命令。

若要抓取類別的每個使用者稽核原則和特定子類別目錄，以針對 Guest 帳戶的系統類別下報告該子類別的包含與排除設定，請輸入：

```
auditpol /get /user:guest /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

若要顯示報告格式的輸出，並包含電腦名稱稱、原則目標、子類別目錄、子類別目錄 GUID、包含設定和排除設定，請輸入：

```
auditpol /get /user:guest /category:detailed Tracking /r
```

若要取得系統類別目錄和子類別目錄的原則，以報告系統稽核原則的分類和子類別目錄原則設定，請輸入：

```
auditpol /get /category:System /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

若要以報告格式抓取詳細追蹤分類和子類別的原則，並包含電腦名稱稱、原則目標、子類別目錄、子類別目錄 GUID、包含設定和排除設定，請輸入：

```
auditpol /get /category:detailed Tracking /r
```

若要取得兩個類別的原則，並將類別指定為 Guid，這會報告兩個類別下所有子類別目錄的所有稽核原則設定，請輸入：

```
auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
```

若要取得 AuditBaseObjects 選項的已啟用或停用狀態，請輸入：

```
auditpol /get /option:AuditBaseObjects
```

可用的選項為 AuditBaseObjects、AuditBaseOperations 和 FullprivilegeAuditing。 若要取得已啟用、已停用或2個 CrashOnAuditFail 選項的狀態，請輸入：

```
auditpol /get /option:CrashOnAuditFail /r
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
