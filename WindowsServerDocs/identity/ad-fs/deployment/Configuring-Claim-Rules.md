---
description: 深入瞭解：設定宣告規則
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: 設定宣告規則
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 70d3c1f562d4faf9cea6608be9e2825dd39e8744
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044167"
---
# <a name="configuring-claim-rules"></a>設定宣告規則

在宣告式身分 \- 識別模型中，Active Directory 同盟服務 \( AD FS \) 為同盟服務的函式是發出包含一組宣告的權杖。 宣告規則會針對 AD FS 問題的宣告來管理決定。 宣告規則和所有伺服器設定資料都會儲存在 AD FS 設定資料庫中。

AD FS 會根據以宣告形式提供的身分識別資訊和其他內容資訊，進行發佈決策。 概括而言，AD FS 藉由將一組宣告做為輸入、執行一些轉換，然後傳回一組不同的宣告作為輸出，以規則處理器的形式運作。

-   [建立規則以傳遞或篩選傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)

-   [建立規則允許所有使用者](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)

-   [建立規則以傳送 AD FS 1.x 相容的宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)

-   [建立規則依據傳入宣告允許或拒絕使用者](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)

-   [建立規則以傳送 LDAP 屬性作為宣告](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)

-   [建立規則以宣告方式傳送群組成員資格](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)

-   [建立規則轉換傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)

-   [建立規則傳送驗證方法相容宣告](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)

-   [建立規則使用自訂規則傳送宣告](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)

## <a name="additional-references"></a>其他參考資料

[AD FS 操作](../ad-fs-operations.md)
