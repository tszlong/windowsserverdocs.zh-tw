---
title: auditpol set
description: 適用於 Windows 命令主題**auditpol set** -設定每個使用者稽核原則、 系統稽核原則，或稽核選項。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8778401efb272a167aaa3d9abb4ecafc67e5f50d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435108"
---
# <a name="auditpol-set"></a>auditpol set

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設定每個使用者稽核原則、 系統稽核原則，或稽核選項。

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
## <a name="parameters"></a>參數

|  參數   |                                                                                                                                          描述                                                                                                                                           |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /user     |                                        設定針對每個使用者稽核原則類別目錄或子類別目錄所指定的安全性主體。 必須指定類別或子類別目錄的選項，為安全性識別元 (SID) 或名稱。                                         |
|   包含   | 指定了 /user;表示使用者的每個使用者原則，將會導致即使它不由系統稽核原則產生的稽核。 這項設定是預設值，並會自動套用，如果既未 / 包含也 /exclude 參數明確指定。 |
|   /exclude   |                                指定了 /user;表示使用者的每位使用者的原則會稽核隱藏不論系統稽核原則。 使用者是本機 Administrators 群組的成員，會忽略此設定。                                |
|  /category   |                                                                            全域唯一識別碼 (GUID) 或名稱所指定的一或多個稽核類別。 如果未不指定任何使用者，會設定系統原則。                                                                             |
| /subcategory |                                                                                         一或多個稽核子類別的 GUID 或名稱所指定。 如果未不指定任何使用者，會設定系統原則。                                                                                          |
|   /success   |                 指定成功稽核。 此設定是預設值，並會自動套用，如果收錄 /success 或 /failure 參數明確指定。 此設定必須搭配參數，指出是否要啟用或停用此設定。                 |
|   /failure   |                                                                                  指定失敗稽核。 此設定必須搭配參數，指出是否要啟用或停用此設定。                                                                                   |
|   /option    |                                                                                   設定 CrashOnAuditFail、 FullprivilegeAuditing、 AuditBaseObjects，還是 AuditBasedirectories 選項的稽核原則。                                                                                    |
|     /sd      |                 設定用來委派存取稽核原則的安全性描述元。 必須使用 Security Descriptor Definition Language (SDDL) 指定的安全性描述元。 安全性描述元必須判別存取控制清單 (DACL)。                 |
|      /?      |                                                                                                                              在命令提示字元顯示說明。                                                                                                                              |

## <a name="remarks"></a>備註
為每位使用者的原則和系統原則的所有設定作業，您必須撰寫或該物件上的完全控制權限設定中的安全性描述元。 您也可以執行設定作業所擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 不過，此權限可讓其他不需要執行 set 作業的存取。
## <a name="BKMK_examples"></a>範例
### <a name="examples-for-the-per-user-audit-policy"></a>每個使用者稽核原則的範例
若要設定每個使用者稽核使用者 mikedan 詳細追蹤類別下的所有子類別目錄的原則，讓所有使用者的成功的嘗試將會稽核，類型：
```
auditpol /set /user:mikedan /category:"detailed Tracking" /include /success:enable
```
若要設定每個使用者稽核原則類別目錄指定名稱和 GUID，以及所要隱藏任何成功或失敗的嘗試次數的稽核 GUID 指定的子類別，請輸入：
```
auditpol /set /user:mikedan /exclude /category:"Object Access","System",{6997984b-797a-11d9-bed3-505054503030} 
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```
若要設定的每個使用者稽核原則中指定之使用者的成功的嘗試以外的所有稽核的歸併的所有類別目錄，請輸入：
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```
### <a name="examples-for-the-system-audit-policy"></a>系統稽核原則的範例
若要設定系統稽核原則，以包含只有成功的嘗試稽核功能詳細的 [追蹤] 類別下的所有子類別目錄，請輸入：
```
auditpol /set /category:"detailed Tracking" /success:enable
```
> [!NOTE]
> 失敗的設定不會改變。
> 若要設定物件存取和系統的類別 （這隱含的子類別目錄會列出因為） 和子類別目錄的嘗試失敗的隱藏項目和成功嘗試的稽核 Guid 所指定的系統稽核原則，請輸入：
> ```
> auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
> ```
> ### <a name="example-for-auditing-options"></a>範例中的稽核選項
> 若要設定的稽核選項為 CrashOnAuditFail 選項的啟用狀態，請輸入：
> ```
> auditpol /set /option:CrashOnAuditFail /value:enable
> ```
> #### <a name="additional-references"></a>其他參考資料
> [命令列語法關鍵](command-line-syntax-key.md)
