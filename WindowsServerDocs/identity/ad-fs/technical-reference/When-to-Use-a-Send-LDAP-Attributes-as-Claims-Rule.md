---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: 使用「以宣告方式傳送 LDAP 屬性」規則的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bc7b98a5d65bf03eb11ed57a8a5f9a0ea696df19
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853771"
---
# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>使用「以宣告方式傳送 LDAP 屬性」規則的時機
當您想要將包含實際輕量型目錄存取協定的輸出宣告 \(屬性存放區中存在的 LDAP\) 屬性值，然後與每個 LDAP 屬性產生關聯時，您可以在 Active Directory 同盟服務 \(AD FS\) 中使用此規則。 如需屬性存放區的詳細資訊，請參閱[屬性存放區的角色](The-Role-of-Attribute-Stores.md)。  
  
使用這項規則時，您需要對您指定且符合規則邏輯的每一個 LDAP 屬性發出宣告，如下表所述。  
  
|規則選項|規則邏輯|  
|---------------|--------------|  
|LDAP 屬性與傳出宣告類型的對應|如果屬性存放區等於*指定的屬性存放區*，且 LDAP 屬性等於*指定的值*，則將 LDAP 屬性值對應至*指定的連出宣告*類型，並發出宣告。|  
  
下列各節提供宣告規則的基本介紹。 另還針對「以宣告方式傳送 LDAP 屬性」規則的時機提供詳細資料。  
  
## <a name="about-claim-rules"></a>關於宣告規則  
宣告規則代表會接受傳入宣告的商務邏輯實例、將條件套用至其 \(如果 x then y\) 並根據條件參數產生傳出宣告。 在您進一步閱讀本主題之前，請先閱讀下列清單，其中會概述關於宣告規則您應該知道的重要秘訣：  
  
-   在的 [AD FS 管理] 嵌入式管理單元\-中，只能使用宣告規則範本建立宣告規則  
  
-   宣告規則會直接從宣告 \(提供者（例如 Active Directory 或其他同盟服務\)，或從宣告提供者信任的接受轉換規則輸出）處理傳入宣告。  
  
-   宣告規則的處理方式，是由宣告發行引擎依時間先後順序按照指定的規則集處理。 藉由設定規則優先順序，您可以進一步精簡或篩選由特定規則集內的上一個規則所產生的宣告。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則和同一宣告類型處理多個宣告值。  
  
如需宣告規則和宣告規則集的詳細資訊，請參閱宣告[規則的角色](The-Role-of-Claim-Rules.md)。 如需如何處理規則的詳細資訊，請參閱[宣告引擎的角色](The-Role-of-the-Claims-Engine.md)。 如需宣告規則集處理方式的詳細資訊，請參閱[宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>LDAP 屬性與傳出宣告類型的對應  
當您使用 [以宣告方式傳送 LDAP 屬性] 規則範本時，您可以從 LDAP 屬性存放區中選取屬性，例如 Active Directory 或 Active Directory Domain Services \(AD DS\)，以宣告方式將其值傳送至信賴憑證者。 這實際上會從您定義的屬性存放區中，將特定的 LDAP 屬性對應至一組可用於授權的連出宣告。  
  
使用這個範本時，您可以從單一規則新增多個屬性，這些屬性將以多個宣告的方式傳送。 比方說，您可以使用這個規則範本建立規則，此規則會從 **company** 和 **department** Active Directory 屬性中，查詢已驗證的使用者的屬性值，然後以兩個不同連出宣告的方式傳送這些值。  
  
您也可以使用此規則來傳送所有使用者的群組成員資格。 如果您只想要傳送個別的群組成員資格，請使用「以宣告方式傳送群組成員資格」規則範本。 如需詳細資訊，請參閱 [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。  
  
## <a name="how-to-create-this-rule"></a>如何建立此規則  
您可以使用宣告規則語言，或使用中的 [AD FS 管理] 嵌入式\-管理單元中的 [以宣告方式傳送 LDAP 屬性] 規則範本來建立此規則。 這個規則範本提供下列設定選項：  
  
-   指定宣告規則名稱  
  
-   選取要從中擷取 LDAP 屬性的屬性存放區  
  
-   LDAP 屬性與傳出宣告類型的對應  
  
如需有關如何建立此規則的詳細資訊，請參閱[建立規則以傳送 LDAP 屬性作為宣告](https://technet.microsoft.com/library/dd807115.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用宣告規則語言  
如果 Active Directory、AD DS 或 Active Directory 輕量型目錄服務 \(AD LDS\) 的查詢必須與**samAccountname**以外的 LDAP 屬性進行比較，您必須改用自訂規則。 如果沒有輸入集中沒有 Windows 帳戶名稱宣告，您也必須使用自訂規則指定要用於查詢 AD DS 或 AD LDS 的宣告。  
  
下列範例協助您了解有各種方式可讓您使用宣告規則語言建構自訂規則，以查詢及擷取屬性存放區中的資料。  
  
### <a name="example-how-to-query-an-adlds-attribute-store-and-return-a-specified-value"></a>範例：如何查詢 AD LDS 屬性存放區並傳回指定的值  
參數必須以分號分隔。 第一個參數是 LDAP 篩選器。 後續參數是在任何相符物件上要傳回的屬性。  
  
下列範例示範如何使用使用者的 mail 屬性值，依據**sAMAccountName**屬性查閱使用者，併發出電子\-郵寄地址宣告：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
下列範例示範如何使用使用者的**Title**和**displayname**屬性值，依據**mail**屬性來查詢使用者，以及發行標題和顯示名稱宣告：  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
下列範例示範如何依郵件和標題查詢使用者，然後使用使用者的**displayname**屬性發出顯示名稱宣告：  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-and-return-a-specified-value"></a>範例：如何查詢 Active Directory 屬性存放區並傳回指定的值  
Active Directory 查詢必須包含使用者名稱 \(並以功能變數名稱\) 做為最終參數，讓 Active Directory 屬性存放區可以查詢正確的網域。 否則支援相同的語法。  
  
下列範例示範如何依用者網域中的 **sAMAccountName** 屬性查詢使用者，然後傳回 **mail** 屬性：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-activedirectory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>範例：如何根據傳入宣告的值查詢 Active Directory 屬性存放區  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
上一個查詢是由下列三個部分所組成：  
  
-   LDAP 篩選器 — 指定這個查詢部分是為了擷取您想要查詢屬性的物件。 如需有效 LDAP 查詢的一般資訊，請參閱 RFC 2254。 當您查詢 Active Directory 屬性存放區，但未指定 LDAP 篩選器時，會假設為 samAccountName\={0} 查詢，而且 Active Directory 屬性存放區預期可以為 {0}饋送值的參數。 否則，查詢會產生錯誤。 對於 Active Directory 以外的 LDAP 屬性存放區，您不能省略查詢的 LDAP 篩選器部分，否則查詢會產生錯誤。  
  
-   屬性規格-在查詢的第二個部分中，您可以指定 \(的屬性，如果您使用多個屬性值\) 您想要的篩選物件外，則會以逗號\-分隔。 您指定的屬性數目必須符合您在查詢中定義的宣告類型數目。  
  
-   Active Directory 網域 — 只有當屬性存放區是 Active Directory 時，才需要指定查詢的最後一部分 \(當您查詢其他屬性存放區時，不需要這麼做。\) 查詢的這個部分是用來指定格式為 domain\\name 的使用者帳戶。 Active Directory 屬性存放區使用「網域」部分來決定要連接的適當網域控制站，然後執行查詢和要求屬性。  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-activedirectory"></a>範例：如何使用兩個自訂規則，從中的屬性將管理員電子\-郵件解壓縮 Active Directory  
當下列兩個自訂規則與下面所示的順序一起使用時，會針對使用者帳戶 \(規則 1\) 的**manager**屬性查詢 Active Directory，然後使用該屬性來查詢管理員的使用者帳戶，\(規則 2\)中的**mail**屬性。 最後， **mail**屬性是以 "ManagerEmail" 宣告的形式發行。 總而言之，Rule 1 查詢 Active Directory 並將查詢的結果傳遞至 Rule 2，然後將管理員 e\-mail 值解壓縮。  
  
例如，當這些規則完成執行時，會發出宣告，其中包含 corp.fabrikam.com 網域中使用者的電子\-郵寄地址。  
  
**規則1**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**規則2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> 只有當使用者的經理位於與此範例\)的使用者 \(corp.fabrikam.com 相同的網域時，這些規則才會生效。  
  
## <a name="additional-references"></a>其他參考資料  
[建立規則來以宣告方式傳送 LDAP 屬性](https://technet.microsoft.com/library/dd807115.aspx)  
  

