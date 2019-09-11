---
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: 使用「以宣告方式傳送群組成員資格」規則的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 546507254f796e6a2fbe71e3ba30a7597ea51295
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869267"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>使用「以宣告方式傳送群組成員資格」規則的時機
當您想要只針對屬於\(指定\)之 Active Directory 安全性群組成員的使用者發出新的傳出宣告值時，可以在 Active Directory 同盟服務 AD FS 中使用此規則。 使用這項規則時，您只需針對您指定且符合規則邏輯的群組發出單一宣告，如下表所述。  
  
|規則選項|規則邏輯|  
|---------------|--------------|  
|傳出宣告值|如果使用者的群組成員資格等於*指定的群組*，且傳出宣告類型等於指定的宣告*類型*，則以指定的*傳出宣告值*取代現有的組名值，併發出宣告。|  
  
下列各節提供宣告規則的基本介紹。 另還針對使用「以宣告方式傳送群組成員資格」規則的時機提供詳細資料。  
  
## <a name="about-claim-rules"></a>關於宣告規則  
宣告規則代表會接受傳入宣告的商務邏輯實例， \(如果 x then y\)則套用條件，並根據條件參數產生傳出宣告。 在您進一步閱讀本主題之前，請先閱讀下列清單，其中會概述關於宣告規則您應該知道的重要秘訣：  
  
-   在 [AD FS 管理]\-嵌入式管理單元中，只能使用宣告規則範本建立宣告規則  
  
-   宣告規則會直接從宣告提供者\(（例如 Active Directory 或另一個同盟服務\) ，或從宣告提供者信任的接受轉換規則輸出）處理傳入宣告。  
  
-   宣告規則的處理方式，是由宣告發行引擎依時間先後順序按照指定的規則集處理。 藉由設定規則優先順序，您可以進一步精簡或篩選由特定規則集內的上一個規則所產生的宣告。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則和同一宣告類型處理多個宣告值。  
  
如需宣告規則和宣告規則集的詳細資訊，請參閱宣告[規則的角色](The-Role-of-Claim-Rules.md)。 如需如何處理規則的詳細資訊，請參閱[宣告引擎的角色](The-Role-of-the-Claims-Engine.md)。 如需宣告規則集處理方式的詳細資訊，請參閱[宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="outgoing-claim-value"></a>傳出宣告值  
使用「以宣告方式傳送群組成員資格」規則範本，您可以發出視使用者是否為您指定群組成員而定的宣告。  
  
換句話說，只有當使用者的群組安全識別碼\(SID\)符合系統管理員指定的 Active Directory 群組時，此規則範本才會發出宣告。 針對 Active Directory Domain Services \(AD DS\)進行驗證的所有使用者，都會有其所屬之每個群組的傳入群組 SID 宣告。 根據預設，Active Directory 宣告提供者信任中的接受轉換規則會傳遞這些群組 SID 宣告。 使用這些群組 Sid 做為發行宣告的基礎，比在 AD DS 中查閱使用者的群組快很多。  
  
使用這項規則時，根據您選取的 Active Directory 群組，只會傳送一個宣告。 比方說，您可以使用這個規則範本建立以下規則：如果使用者是 Domain Admins 安全性群組的成員，則會傳送值為 "Admin" 的群組宣告。  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>在宣告提供者信任上設定此規則  
只有當從宣告提供者收到群組 SID 時 (這個情況對任何宣告提供者而言並不常見，Active Directory 或 AD DS 除外)，系統管理員應該在宣告提供者信任的接受轉換規則中使用這個規則類型。  
  
## <a name="how-to-create-this-rule"></a>如何建立此規則  
您可以使用宣告規則語言，或使用 [AD FS 管理] 嵌入式管理單元\-中的 [以宣告方式傳送 LDAP 群組成員資格] 規則範本來建立此規則。 這個規則範本提供下列設定選項：  
  
-   指定宣告規則名稱  
  
-   使用物件選擇器選取使用者的群組  
  
-   選取傳出宣告類型  
  
-   選取 [外寄名稱識別碼\(] 格式，只有在從 [傳出宣告類型] 欄位中選擇 [名稱識別碼] 時才能使用\)  
  
-   指定傳出宣告值  
  
如需有關如何建立此規則的詳細資訊，請參閱[建立規則以宣告方式傳送群組成員資格](https://technet.microsoft.com/library/ee913569.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用宣告規則語言  
如果您想要根據傳入 SID (而不是群組 SID) 發行宣告，請使用「轉換傳入宣告」規則範本。 如果系統管理員想要擷取使用者為其成員的所有群組名稱，請使用「以宣告方式傳送 LDAP 屬性」規則範本，而不是使用 **tokenGroups** 屬性。  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>範例：如何根據使用者的群組成員資格發出群組宣告  
下列規則會根據連入群組 SID 為使用者發出群組宣告：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>其他參考資料  
[建立規則來以宣告方式傳送 LDAP 屬性](https://technet.microsoft.com/library/dd807115.aspx)  
  

