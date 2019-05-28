---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: 設定宣告規則
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d0dd8528e5fbd6829b313a3e6bc47f5a17f6a12f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189722"
---
# <a name="configure-claim-rules"></a>設定宣告規則

在 宣告\-型的識別模型中，Active Directory Federation Services 的函式\(AD FS\)為同盟服務是以發出包含一組宣告的權杖。 宣告規則可管理 AD FS 所發出的宣告方面所做的決策。 宣告規則和資料會儲存在 AD FS 設定資料庫的所有伺服器組態。  
  
AD FS 進行身分識別資訊提供給它的宣告形式和其他內容資訊為基礎的發行決策。 概括而言，AD FS 運作所採用一個規則處理器的宣告集做為輸入，執行轉換，重試次數，並再傳回一組不同的宣告做為輸出。 

下列主題將協助您建立 AD FS 會處理規則： 
  
-   [建立規則來傳遞或篩選傳入宣告](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [建立規則允許所有使用者](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [建立規則依據傳入宣告允許或拒絕使用者](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [建立規則，以宣告形式傳送 LDAP 屬性](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [建立規則以宣告方式傳送群組成員資格](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [建立規則轉換傳入宣告](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [建立規則傳送驗證方法相容宣告](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [建立規則，以傳送 AD FS 1.x 相容宣告](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [建立規則使用自訂規則傳送宣告](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
