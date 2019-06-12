---
title: auditpol get
description: 適用於 Windows 命令主題**auditpol 取得**-擷取系統原則、 稽核選項，以及稽核安全性描述元物件的每個使用者原則。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe13de4e-836c-4207-b47c-64b6272d6c41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03ba59b19af42ab2d3fdd1dd52d976d381779640
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435141"
---
# <a name="auditpol-get"></a>auditpol get

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

擷取系統原則、 稽核選項，以及稽核安全性描述元物件的每個使用者原則。

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
## <a name="parameters"></a>參數

|  參數   |                                                                                                                                         描述                                                                                                                                          |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /user     | 顯示查詢的每個使用者稽核原則的安全性主體。 必須指定 /category 或 /subcategory 參數。 使用者可能會指定為安全識別項 (SID) 或名稱。 如果使用者未不指定任何帳戶，然後會查詢系統稽核原則。 |
|  /category   |                                                          全域唯一識別碼 (GUID) 或名稱所指定的一或多個稽核類別。 星號 (\*) 可用來指出應該要查詢所有的稽核類別目錄。                                                          |
| /subcategory |                                                                                                                  一或多個稽核子類別的 GUID 或名稱所指定。                                                                                                                  |
|     /sd      |                                                                                                        擷取用來委派存取稽核原則的安全性描述元。                                                                                                        |
|   /option    |                                                                              擷取現有的原則 CrashOnAuditFail、 FullprivilegeAuditing、 AuditBaseObjects，還是 AuditBasedirectories 選項。                                                                               |
|      /r      |                                                                                                              在 報表格式，以逗號分隔值 (CSV)，會顯示輸出。                                                                                                              |
|      /?      |                                                                                                                             在命令提示字元顯示說明。                                                                                                                             |

## <a name="remarks"></a>備註
所有類別和子類別可以被都指定的 GUID 或用引號括住的名稱。 指定使用者的 SID 或名稱。
每位使用者的原則和系統原則的所有 get 作業，您必須擁有都讀取權限的安全性描述元中設定該物件。 您也可以執行 get 作業擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 不過，此權限可讓其他不需要執行取得作業的存取。
## <a name="BKMK_examples"></a>範例
### <a name="examples-for-the-per-user-audit-policy"></a>每個使用者稽核原則的範例
擷取 Guest 帳戶的每個使用者稽核原則，並針對系統，顯示輸出的詳細追蹤，以及物件存取類別目錄中，輸入：
```
auditpol /get /user:{S-1-5-21-1443922412-3030960370-963420232-51} /category:"System","detailed Tracking","Object Access"
```
> [!NOTE]
> 此命令可用於兩種案例。 當監視可疑活動的特定使用者帳戶，您可以使用 /get 命令來擷取特定類別中的結果，使用包含原則以啟用其他稽核。 或者，如果帳戶上的稽核設定會記錄許多但非必要的事件，您可以使用 /get 命令來篩選掉無關的事件，該帳戶與排除原則。 如需所有類別目錄中，使用 auditpol /list /category 命令。
> 若要擷取每個使用者稽核原則中的類別和特定的子類別，它會報告該來賓帳戶的 [系統] 類別下的子類別目錄的內含和專有的設定，請輸入：
> ```
> auditpol /get /user:guest /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> 若要顯示在 報表格式的輸出，並包含電腦名稱、 原則目標、 子類別目錄、 子類別目錄 GUID，包含設定及排除設定，請輸入：
> ```
> auditpol /get /user:guest /category:detailed Tracking" /r
> ```
> ### <a name="examples-for-the-system-audit-policy"></a>系統稽核原則的範例
> 若要擷取的原則的系統分類和子類別，它會報告系統稽核原則類別目錄和子類別目錄原則設定，請輸入：
> ```
> auditpol /get /category:"System" /subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> 若要擷取詳細的追蹤分類和子類別目錄報表格式的原則，並包含電腦名稱、 原則目標、 子類別目錄、 子類別目錄 GUID，包含設定及排除設定類型：
> ```
> auditpol /get /category:"detailed Tracking" /r
> ```
> 若要擷取與類別的兩個類別目錄原則指定為 Guid，它會報告所有的稽核原則設定的所有子類別目錄在兩個類別，型別：
> ```
> auditpol /get /category:{69979849-797a-11d9-bed3-505054503030},{69997984a-797a-11d9-bed3-505054503030} subcategory:{0ccee921a-69ae-11d9-bed3-505054503030}
> ```
> ### <a name="examples-for-auditing-options"></a>適用於稽核選項範例
> 若要擷取狀態，啟用或停用，AuditBaseObjects 選項中，輸入：
> ```
> auditpol /get /option:AuditBaseObjects
> ```
> [!NOTE]
> 可用的選項為 AuditBaseObjects、 AuditBaseOperations 和 FullprivilegeAuditing。
> 若要擷取已啟用的狀態，請停用或 2 的 CrashOnAuditFail 選項中，輸入：
> ```
> auditpol /get /option:CrashOnAuditFail /r
> ```
> #### <a name="additional-references"></a>其他參考資料
> [命令列語法關鍵](command-line-syntax-key.md)
