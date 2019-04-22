---
title: auditpol 移除
description: 適用於 Windows 命令主題**auditpol 移除**-移除指定的帳戶或所有帳戶的每個使用者稽核原則。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 566827e93d9f8c9dc0d00f4f704513369fbb44ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818299"
---
# <a name="auditpol-remove"></a>auditpol 移除

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

移除指定的帳戶或所有帳戶的每個使用者稽核原則。

## <a name="syntax"></a>語法
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/user|指定的安全性識別碼 (SID)，或針對每個使用者稽核原則之使用者的使用者名稱會被刪除。|
|/allusers|移除所有使用者的每個使用者稽核原則。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
針對每個使用者原則的移除作業，您必須寫入 」 或 「 完全控制 」 權限的安全性描述元中設定該物件。 您也可以執行移除作業擁有**管理稽核及安全性記錄**(SeSecurityPrivilege) 使用者權限。 不過，此權限可讓其他不需要執行的移除作業的存取。
## <a name="BKMK_examples"></a>範例
若要依名稱移除使用者 mikedan 的每個使用者稽核原則，請輸入：
```
auditpol /remove /user:mikedan
```
若要移除使用者 mikedan sid 的每個使用者稽核原則，請輸入：
```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```
若要移除所有使用者的每個使用者稽核原則，請輸入：
```
auditpol /remove /allusers
```
#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](command-line-syntax-key.md)
