---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: 設定宣告規則
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 259e2b266b64a3b34c237cfe209a3558124c8ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871629"
---
# <a name="configure-claim-rules"></a>設定宣告規則

>適用於：Windows Server 2016, Windows Server 2012 R2

在 宣告\-型的識別模型為同盟服務的 Active Directory Federation Services (AD FS) 的函式是在發行包含一組宣告的權杖。 宣告規則可管理 AD FS 所發出的宣告方面所做的決策。 宣告規則和資料會儲存在 AD FS 設定資料庫的所有伺服器組態。  
  
AD FS 進行身分識別資訊提供給它的宣告形式和其他內容資訊為基礎的發行決策。 概括而言，AD FS 運作所採用一個規則處理器的宣告集做為輸入，執行轉換，重試次數，並再傳回一組不同的宣告做為輸出。 

下列主題將協助您建立 AD FS 會處理規則： 
  
-   [建立規則來傳遞或篩選傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [建立規則以允許所有使用者](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [建立規則以允許或拒絕使用者根據傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [建立規則，以宣告形式傳送 LDAP 屬性](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [建立規則，以宣告形式傳送群組成員資格](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [建立規則來轉換傳入宣告](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [建立要傳送的驗證方法宣告規則](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [建立規則，以使用自訂規則傳送宣告](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
