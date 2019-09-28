---
title: auditpol remove
description: '**Auditpol remove**的 Windows 命令主題-移除指定帳戶或所有帳戶的每個使用者稽核原則。'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2d652cb3bf7156bc1b1198616c9f6e57f588acd8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382476"
---
# <a name="auditpol-remove"></a>auditpol remove

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

移除指定帳戶或所有帳戶的每個使用者稽核原則。

## <a name="syntax"></a>語法
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>參數
|參數|描述|
|-------|--------|
|/user|針對要刪除其每個使用者稽核原則的使用者，指定其安全識別碼（SID）或使用者名稱。|
|/allusers|移除所有使用者的每個使用者稽核原則。|
|/?|在命令提示字元顯示說明。|
## <a name="remarks"></a>備註
針對每個使用者原則的移除作業，您必須在安全描述項中設定該物件的寫入或完全控制許可權。 您也可以透過擁有 [**管理審核及安全性記錄檔**] （SeSecurityPrivilege）使用者權限來執行移除作業。 不過，此許可權允許執行移除作業所不需要的其他存取權。
## <a name="BKMK_examples"></a>典型
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
#### <a name="additional-references"></a>其他參考
[命令列語法關鍵](command-line-syntax-key.md)
