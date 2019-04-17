---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: "使用傳送群組成員資格理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10e027b4de463aad2b05eae40a622aebf8f12a3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>使用傳送群組成員資格理賠要求規則
當您想要發行成員指定 Activ Directory 安全性群組的這些使用者新傳出宣告值，您可以使用在 Active Directory 同盟服務 \(AD FS\) 本規則。 當您使用此規則時，您發出僅限群組單一理賠要求您指定和符合規則邏輯操作，下列表格中所述。  
  
|規則選項|邏輯規則|  
|---------------|--------------|  
|傳出宣告值|如果使用者的群組成員資格等於*指定的群組*和傳出宣告類型等於*指定理賠要求輸入*，然後取代現有的群組名稱值為*指定傳出宣告值*並發出理賠要求。|  
  
下列章節提供基本簡介取得規則。 它們也提供使用傳送群組成員資格理賠要求規則詳細資訊。  
  
## <a name="about-claim-rules"></a>關於理賠要求規則  
宣告規則表示商務邏輯操作，將需要連入宣告、 適用於條件的執行個體 \ （如果 x 然後 y\） 和產生傳出宣告依據條件的參數。 下列清單輪廓重要您進一步讀取之前，您必須知道的相關的提示取得規則本主題中：  
  
-   AD FS snap\ 中管理，在理賠要求規則只能建立使用理賠要求規則範本  
  
-   宣告規則處理程序傳入宣告直接從宣告提供者 \ （例如 Active Directory 或其他聯盟 Service\） 或接受的輸出從轉換宣告提供者信任規則。  
  
-   宣告發行引擎順序特定的規則集中處理理賠要求規則。 藉由設定優先順序規則，可以進一步改善或篩選宣告專特定的規則設定中的上一個規則。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則相同宣告類型處理多個理賠要求值。  
  
如需詳細資訊理賠要求規則及宣告規則集合，請查看[的角色的取得規則](The-Role-of-Claim-Rules.md)。 如需規則的處理方式的相關資訊，請查看[的角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。 宣告規則集合的處理方式的相關資訊，請查看[的角色宣告管線的](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="outgoing-claim-value"></a>傳出宣告值  
您可以使用傳送群組成員資格做為理賠要求規則範本，發出的使用者是否指定群組成員意外理賠要求。  
  
囉宣告使用者有群組安全性時，只為此規則範本問題 ID \(SID\) 符合系統管理員指定 Active Directory 群組。 針對 \(AD DS\) Active Directory Domain Services 進行驗證的所有使用者將會都有連入的群組 SID 宣告及其所屬的每個群組。 根據預設，在 Active Directory 宣告提供者信任的接受轉換規則通過 SID 宣告這些群組。 使用做為基礎的發行宣告 Sid 速度更快比群 AD DS 中查看這些群組。  
  
當您使用此規則，只有單一理賠要求傳送，根據您選擇的 Active Directory 群組。 例如，您可以使用此規則範本建立規則，將會傳送群組理賠要求的值為 「 系統管理員 」，如果使用者網域管理安全性群組成員。  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>宣告提供者信任上設定此規則  
系統管理員應該使用此規則類型接受轉換規則宣告提供者信任的中才群組 Sid 會收到宣告提供者，這是很少 Active Directory 或 AD DS 以外的任何宣告提供者。  
  
## <a name="how-to-create-this-rule"></a>如何建立本規則  
建立使用此規則理賠要求規則語言、 來使用理賠要求傳送給 LDAP 群組成員資格規則 snap\ 中 AD FS 管理範本。 此規則範本提供下列設定選項：  
  
-   指定名稱理賠要求規則  
  
-   選取 [使用者群組使用物件選擇器  
  
-   選取傳出宣告類型  
  
-   選取 [傳出名稱 ID 格式 \ （，都可以僅限時名稱 ID 從選擇傳出宣告輸入 field\）  
  
-   指定傳出宣告值  
  
如需如何建立本規則的詳細資訊，請查看[建立規則為理賠要求傳送給群組成員資格以](https://technet.microsoft.com/en-us/library/ee913569.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用語言理賠要求規則  
如果您想要發行宣告根據傳入 SID 群組 SID 以外，使用轉換連入宣告規則範本。 如果系統管理員想要擷取的使用者的成員所有群組的名稱，作為傳送 LDAP 屬性宣告規則範本改為使用**tokenGroups**屬性。  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>範例： 如何發出以使用者的群組成員資格群組宣告  
下列規則問題群組宣告根據連入的群組 SID 使用者：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>其他參考資料  
[建立為宣告傳送 LDAP 屬性規則](https://technet.microsoft.com/library/dd807115.aspx)  
  

