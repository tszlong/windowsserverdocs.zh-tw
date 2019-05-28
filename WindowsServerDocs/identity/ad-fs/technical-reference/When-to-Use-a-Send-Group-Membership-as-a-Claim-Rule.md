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
ms.openlocfilehash: ea537aa61cd7bfbe05ed1dd151eddd4a0bfc5ca7
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188305"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>使用「以宣告方式傳送群組成員資格」規則的時機
您可以使用這項規則中 Active Directory Federation Services \(AD FS\)當您想要發出新的傳出宣告值只有這些使用者是指定的 Active Directory 安全性群組的成員。 使用這項規則時，您只需針對您指定且符合規則邏輯的群組發出單一宣告，如下表所述。  
  
|規則選項|規則邏輯|  
|---------------|--------------|  
|傳出宣告值|如果使用者的群組成員資格等於*指定群組*，且傳出宣告類型等於*指定宣告類型*，則請使用*指定傳出宣告值*來取代現有的群組名稱值，並發出宣告。|  
  
下列各節提供宣告規則的基本介紹。 另還針對使用「以宣告方式傳送群組成員資格」規則的時機提供詳細資料。  
  
## <a name="about-claim-rules"></a>關於宣告規則  
宣告規則表示商務邏輯，會收取傳入宣告，對其套用條件的執行個體\(若 x 則 y\)和產生根據條件參數傳出宣告。 在您進一步閱讀本主題之前，請先閱讀下列清單，其中會概述關於宣告規則您應該知道的重要秘訣：  
  
-   在 AD FS 管理嵌入式管理單元\-，宣告規則只能建立使用宣告規則範本  
  
-   宣告規則處理程序內送宣告直接從宣告提供者\(Active Directory 或其他同盟服務等\)或宣告提供者信任上從輸出的接受轉換規則。  
  
-   宣告規則的處理方式，是由宣告發行引擎依時間先後順序按照指定的規則集處理。 藉由設定規則優先順序，您可以進一步精簡或篩選由特定規則集內的上一個規則所產生的宣告。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則和同一宣告類型處理多個宣告值。  
  
如需詳細的宣告規則和宣告規則集的相關資訊，請參閱[規則的角色宣告](The-Role-of-Claim-Rules.md)。 如需有關規則的處理方式的詳細資訊，請參閱 < [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 宣告規則集的處理方式的詳細資訊，請參閱[The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="outgoing-claim-value"></a>傳出宣告值  
使用「以宣告方式傳送群組成員資格」規則範本，您可以發出視使用者是否為您指定群組成員而定的宣告。  
  
換句話說，此規則範本發出宣告時只有在使用者具有群組安全性識別碼時，才\(SID\)符合系統管理員指定的 Active Directory 群組。 所有對 Active Directory 網域服務進行驗證的使用者\(AD DS\)會有連入群組 SID 宣告它們屬於每個群組。 根據預設，Active Directory 宣告提供者信任中的接受轉換規則會傳遞這些群組 SID 宣告。 與在 AD DS 中查閱使用者的群組相比，使用這些群組 SID 做為發出宣告的基礎要快得多。  
  
使用這項規則時，根據您選取的 Active Directory 群組，只會傳送一個宣告。 比方說，您可以使用這個規則範本建立以下規則：如果使用者是 Domain Admins 安全性群組的成員，則會傳送值為 "Admin" 的群組宣告。  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>在宣告提供者信任上設定此規則  
只有當從宣告提供者收到群組 SID 時 (這個情況對任何宣告提供者而言並不常見，Active Directory 或 AD DS 除外)，系統管理員應該在宣告提供者信任的接受轉換規則中使用這個規則類型。  
  
## <a name="how-to-create-this-rule"></a>如何建立此規則  
您建立此規則使用宣告規則語言，或透過傳送 LDAP 群組成員資格為宣告規則範本，在 AD FS 管理嵌入式管理單元\-中。 這個規則範本提供下列設定選項：  
  
-   指定宣告規則名稱  
  
-   使用物件選擇器選取使用者的群組  
  
-   選取傳出宣告類型  
  
-   選取傳出名稱識別碼格式\(所提供僅當名稱識別碼從選擇 [傳出宣告類型] 欄位\)  
  
-   指定傳出宣告值  
  
如需如何建立此規則的詳細資訊，請參閱[建立規則，以宣告形式傳送群組成員資格](https://technet.microsoft.com/library/ee913569.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用宣告規則語言  
如果您想要根據傳入 SID (而不是群組 SID) 發行宣告，請使用「轉換傳入宣告」規則範本。 如果系統管理員想要擷取使用者為其成員的所有群組名稱，請使用「以宣告方式傳送 LDAP 屬性」規則範本，而不是使用 **tokenGroups** 屬性。  
  
### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>範例：如何根據使用者的群組成員資格發出群組宣告  
下列規則會根據連入群組 SID 為使用者發出群組宣告：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]  
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);  
```  
  
## <a name="additional-references"></a>其他參考資料  
[建立規則，以宣告形式傳送 LDAP 屬性](https://technet.microsoft.com/library/dd807115.aspx)  
  

