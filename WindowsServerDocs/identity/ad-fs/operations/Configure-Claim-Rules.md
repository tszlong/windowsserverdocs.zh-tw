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
ms.openlocfilehash: f9e0509e2f870fd0edc7f0c6a241d789945e7ccb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829409"
---
# <a name="configure-claim-rules"></a>設定宣告規則

>適用於：Windows Server 2016, Windows Server 2012 R2

在 宣告\-型的識別模型中，Active Directory Federation Services 的函式\(AD FS\)為同盟服務是以發出包含一組宣告的權杖。 宣告規則可管理 AD FS 所發出的宣告方面所做的決策。 宣告規則和資料會儲存在 AD FS 設定資料庫的所有伺服器組態。  
  
AD FS 進行身分識別資訊提供給它的宣告形式和其他內容資訊為基礎的發行決策。 概括而言，AD FS 運作所採用一個規則處理器的宣告集做為輸入，執行轉換，重試次數，並再傳回一組不同的宣告做為輸出。 

下列主題將協助您建立 AD FS 會處理規則： 
  
-   [建立規則來傳遞或篩選傳入宣告](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [建立規則以允許所有使用者](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [建立規則以允許或拒絕使用者根據傳入宣告](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [建立規則，以宣告形式傳送 LDAP 屬性](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [建立規則，以宣告形式傳送群組成員資格](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [建立規則來轉換傳入宣告](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [建立要傳送的驗證方法宣告規則](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [建立規則，以傳送 AD FS 1.x 相容宣告](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [建立規則，以使用自訂規則傳送宣告](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>另請參閱  
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md) 
