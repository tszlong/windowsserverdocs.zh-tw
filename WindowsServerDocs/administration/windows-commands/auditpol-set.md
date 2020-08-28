---
title: auditpol set
description: Auditpol set 命令的參考文章，它會設定每個使用者稽核原則、系統稽核原則或審核選項。
ms.topic: reference
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 674de256eba3ee4b55f2b889717b7c2ed2defa3d
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028966"
---
# <a name="auditpol-set"></a>auditpol set

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定每個使用者稽核原則、系統稽核原則或審核選項。

若要對*每個使用者*和*系統*策略執行*設定*作業，您必須擁有安全描述項中該物件集的 [**寫入**] 或 [**完全控制**] 許可權。 如果您有 [**管理審核和安全性記錄**檔 (SeSecurityPrivilege) 使用者權限，也可以執行*設定*作業。 不過，此許可權可讓您不需要執行整體 *設定* 作業的額外存取權。

## <a name="syntax"></a>語法

```
auditpol /set
[/user[:<username>|<{sid}>][/include][/exclude]]
[/category:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/subcategory:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/option:<option name> /value: <enable>|<disable>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /user | 已設定類別或子類別所指定之每個使用者稽核原則的安全性主體。 您必須指定類別或子類別目錄選項，做為 (SID) 或名稱的安全識別碼。 |
| /include | 以/user 指定;指出使用者的每個使用者原則將會產生審核，即使系統稽核原則未指定也是一樣。 這項設定是預設值，如果未明確指定/include 或/exclude 參數，則會自動套用。 |
| /exclude | 以/user 指定;指出不論系統稽核原則為何，使用者的每個使用者原則都會導致隱藏的審核。 針對身為本機 Administrators 群組成員的使用者，系統會忽略此設定。 |
| /類別 | 全域唯一識別碼指定的一或多個審核分類 (GUID) 或名稱。 如果未指定使用者，則會設定系統原則。 |
| /subcategory | GUID 或名稱所指定的一或多個審核子類別。 如果未指定使用者，則會設定系統原則。 |
| /success | 指定成功的審核。 這項設定是預設值，如果未明確指定/success 或/failure 參數，則會自動套用。 此設定必須搭配指出是否要啟用或停用設定的參數使用。 |
| /failure | 指定失敗的審核。 此設定必須搭配指出是否要啟用或停用設定的參數使用。 |
| /option | 設定 CrashOnAuditFail、FullprivilegeAuditing、AuditBaseObjects 或 AuditBasedirectories 選項的稽核原則。 |
| /sd | 設定用來將存取權委派給稽核原則的安全描述項。 您必須使用安全描述項定義語言 (SDDL) 來指定安全描述項。 安全描述項必須有 (DACL) 的任意存取控制清單。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要在使用者 mikedan 的詳細追蹤類別下設定所有子類別的每個使用者稽核原則，以便進行所有使用者的成功嘗試，請輸入：

```
auditpol /set /user:mikedan /category:detailed Tracking /include /success:enable
```

若要針對名稱和 GUID 所指定的類別設定依使用者稽核原則，以及 GUID 指定的子類別來抑制任何成功或失敗嘗試的審核，請輸入：

```
auditpol /set /user:mikedan /exclude /category:Object Access,System,{6997984b-797a-11d9-bed3-505054503030}
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```

若要為指定的使用者設定每個使用者的稽核原則，讓所有類別都能順利進行所有但成功的嘗試，請輸入：
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```

若要針對詳細追蹤類別下的所有子類別設定系統稽核原則，使其只包含成功嘗試的審核，請輸入：

```
auditpol /set /category:detailed Tracking /success:enable
```

> [!NOTE]
> 失敗設定未變更。

若要為 [物件存取] 和 [系統類別目錄] 設定系統稽核原則 (這是隱含的，因為子類別會列出) 和 Guid 指定的子類別，以顯示失敗的嘗試和成功的嘗試，請輸入：

```
auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
```

若要將審核選項設定為 [CrashOnAuditFail] 選項的 [已啟用] 狀態，請輸入：

```
auditpol /set /option:CrashOnAuditFail /value:enable
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [auditpol 命令](auditpol.md)
