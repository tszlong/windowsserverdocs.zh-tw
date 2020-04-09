---
title: auditpol 設定
description: '**Auditpol set**的 Windows 命令主題，可設定每個使用者的稽核原則、系統稽核原則或審核選項。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0773a0a9ae9237b39293bae80001616d00630436
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851141"
---
# <a name="auditpol-set"></a>auditpol 設定

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

設定每位使用者的稽核原則、系統稽核原則或審核選項。

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
| /user | 設定類別或子類別所指定之每個使用者稽核原則的安全性主體。 必須指定 [類別] 或 [子類別] 選項，做為安全識別碼（SID）或名稱。 |
| /include | 使用/user 指定;指出使用者的每個使用者原則，即使系統稽核原則未指定，也會產生 audit。 此設定是預設值，如果未明確指定/include 或/exclude 參數，則會自動套用。 |
| /exclude | 使用/user 指定;指出使用者的每一使用者原則，不論系統稽核原則為何，都會使 audit 遭到抑制。 對於身為本機 Administrators 群組成員的使用者，會忽略此設定。 |
| /category | 全域唯一識別碼（GUID）或名稱所指定的一或多個 audit 分類。 如果未指定任何使用者，則會設定系統原則。 |
| /subcategory | GUID 或名稱所指定的一或多個 audit 子類別。 如果未指定任何使用者，則會設定系統原則。 |
| /success | 指定成功的審核。 此設定是預設值，如果未明確指定/success 或/failure 參數，則會自動套用。 此設定必須與指出是否啟用或停用設定的參數搭配使用。 |
| /failure | 指定失敗的審核。 此設定必須與指出是否啟用或停用設定的參數搭配使用。 |
| /option | 設定 CrashOnAuditFail、FullprivilegeAuditing、AuditBaseObjects 或 AuditBasedirectories 選項的稽核原則。 |
| /sd | 設定用來將存取權委派給稽核原則的安全描述項。 安全描述項必須使用安全描述項定義語言（SDDL）來指定。 安全描述項必須具有任意存取控制清單（DACL）。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

針對每個使用者原則和系統原則的所有設定作業，您必須在安全描述項中設定該物件的「寫入」或「完全控制」許可權。 您也可以透過擁有「**管理審核及安全性記錄檔**」（SeSecurityPrivilege）使用者權限來執行設定作業。 不過，此許可權允許執行設定作業所不需要的其他存取權。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要為使用者 mikedan 的 [詳細追蹤] 類別底下的所有子類別目錄設定個別使用者稽核原則，讓所有使用者成功的嘗試都能通過審核，請輸入：

```
auditpol /set /user:mikedan /category:detailed Tracking /include /success:enable
```

若要針對 [名稱] 和 [GUID] 指定的分類，以及 GUID 指定的子類別來設定每個使用者的稽核原則，以隱藏任何成功或失敗嘗試的審核，請輸入：

```
auditpol /set /user:mikedan /exclude /category:Object Access,System,{6997984b-797a-11d9-bed3-505054503030}
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```

若要針對指定的使用者設定每個使用者的稽核原則，以顯示所有可成功嘗試的所有類別，請輸入：
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```

若要將 [詳細追蹤] 分類底下所有子類別的系統稽核原則設定為僅包含成功嘗試的審核，請輸入：

```
auditpol /set /category:detailed Tracking /success:enable
```

> [!NOTE]
> 失敗設定不會改變。

若要設定物件存取和系統類別的系統稽核原則（隱含的原因是因為列出子類別）和 Guid 指定的子類別，以隱藏嘗試失敗和成功的嘗試，請輸入：

```
auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
```

若要將 [CrashOnAuditFail] 選項的 [審核選項] 設定為 [已啟用] 狀態，請輸入：

```
auditpol /set /option:CrashOnAuditFail /value:enable
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
