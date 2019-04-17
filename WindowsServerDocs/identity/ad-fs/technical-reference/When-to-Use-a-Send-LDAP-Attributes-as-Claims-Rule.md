---
ms.assetid: 606f4196-b579-4806-a462-3abd4d93e87c
title: "使用傳送 LDAP 屬性宣告規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05a2dd88057b64675bbc3bd30724d1eda0880c44
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-send-ldap-attributes-as-claims-rule"></a>使用傳送 LDAP 屬性宣告規則
當您想要發行傳出宣告實際輕量型 Directory 存取通訊協定 \(LDAP\) 屬性值的存在屬性市集中，然後關聯宣告類型每個 LDAP 屬性，您可以使用在 Active Directory 同盟服務 \(AD FS\) 本規則。 如需屬性存放區的詳細資訊，請查看[的角色的屬性儲存](The-Role-of-Attribute-Stores.md)。  
  
當您使用此規則時，您發出理賠要求的每個 LDAP 屬性，指定和符合規則邏輯操作，下列表格中所述。  
  
|規則選項|邏輯規則|  
|---------------|--------------|  
|傳出宣告類型 LDAP 屬性的對應|如果屬性市集等於*屬性指定網上商店*和 LDAP 屬性等於*指定值*，然後地圖 LDAP 屬性值來*指定傳出宣告*鍵入和發出理賠要求。|  
  
下列章節提供基本簡介取得規則。 它們也提供使用傳送 LDAP 屬性宣告規則詳細資訊。  
  
## <a name="about-claim-rules"></a>關於理賠要求規則  
宣告規則表示商務邏輯操作，將需要連入宣告、 適用於條件的執行個體 \ （如果 x 然後 y\） 和產生傳出宣告依據條件的參數。 下列清單輪廓重要您進一步讀取之前，您必須知道的相關的提示取得規則本主題中：  
  
-   AD FS snap\ 中管理，在理賠要求規則只能建立使用理賠要求規則範本  
  
-   宣告規則處理程序傳入宣告直接從宣告提供者 \ （例如 Active Directory 或其他聯盟 Service\） 或接受的輸出從轉換宣告提供者信任規則。  
  
-   宣告發行引擎順序特定的規則集中處理理賠要求規則。 藉由設定優先順序規則，可以進一步改善或篩選宣告專特定的規則設定中的上一個規則。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則相同宣告類型處理多個理賠要求值。  
  
如需詳細資訊理賠要求規則及宣告規則集合，請查看[的角色的取得規則](The-Role-of-Claim-Rules.md)。 如需規則的處理方式的相關資訊，請查看[的角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。 宣告規則集合的處理方式的相關資訊，請查看[的角色宣告管線的](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="mapping-of-ldap-attributes-to-outgoing-claim-types"></a>傳出宣告類型 LDAP 屬性的對應  
當您使用傳送 LDAP 屬性為宣告規則範本時，您可以從 LDAP 屬性市集中，例如 Active Directory 或 Active Directory Domain Services \(AD DS\) 將它們的值為宣告信賴來選取屬性。 基本上這地圖服務屬性網上商店，您可以用於授權傳出宣告一組定義特定 LDAP 屬性。  
  
使用此範本，您可以新增多個屬性，這將會以多個宣告傳送，從單一規則。 例如您可以使用此規則範本建立規則的樣子屬性的值驗證使用者從**公司**和**部門**Active Directory 屬性，並為有兩個不同的傳出宣告將這些值。  
  
您也可以使用此規則傳送給所有使用者的群組成員資格。 如果您想要傳送僅限個人群組成員資格，作為理賠要求規則範本傳送群組成員資格。 如需詳細資訊，請查看[使用傳送群組成員資格理賠要求規則以](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。  
  
## <a name="how-to-create-this-rule"></a>如何建立本規則  
您可以使用理賠要求規則語言建立此規則或藉由傳送 LDAP 屬性主張使用規則 snap\ 中 AD FS 管理範本。 此規則範本提供下列設定選項：  
  
-   指定名稱理賠要求規則  
  
-   選取要從中解壓縮 LDAP 屬性屬性網上商店  
  
-   傳出宣告類型 LDAP 屬性的對應  
  
如需如何建立本規則的詳細資訊，請查看[建立規則為宣告傳送 LDAP 屬性，](https://technet.microsoft.com/library/dd807115.aspx)。  
  
## <a name="using-the-claim-rule-language"></a>使用語言理賠要求規則  
如果查詢 Active Directory、 AD DS 或 Active Directory 輕量型 Directory 服務 \(AD LDS\) 必須比較 LDAP 屬性以外**samAccountname**，您必須改用自訂規則。 如果有任何 Windows Account 名稱宣告輸入設定中的，您必須也使用自訂規則指定使用查詢 AD DS 或廣告 LDS 理賠要求。  
  
以下被提供的範例可協助您了解您可以建立自訂規則使用理賠要求規則語言查詢和解壓縮資料屬性市集中的各種方式。  
  
### <a name="example-how-to-query-an-ad-lds-attribute-store-and-return-a-specified-value"></a>範例： 如何查詢 AD LDS 屬性市集，並傳回指定的值  
必須分號來分隔參數。 第一次參數是 LDAP 篩選。 後續參數是返回任何比對物件的屬性。  
  
下例示範如何查看使用者透過**sAMAccountName**屬性，並發出 e\-電子郵件地址宣告，請使用使用者的電子郵件屬性的值：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"));  
  
```  
  
下例示範如何查看使用者透過**郵件**屬性，並發出標題和顯示名稱宣告，使用的值使用者的**標題**並**顯示名稱**屬性：  
  
```  
c:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress ", Issuer == "AD AUTHORITY"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "mail={0};title;displayname", param = c.Value);  
```  
  
下例示範如何查看使用者透過電子郵件和標題和再發行 [顯示名稱理賠要求使用使用者的**顯示名稱**屬性：  
  
```  
c1:[Type == " http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"] && c2:[Type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/title"]  
=> issue(store = "AD LDS ", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/displayname"), query = "(&(mail={0})(title={1}));displayname", param = c1.Value, param = c2.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-and-return-a-specified-value"></a>範例： 如何查詢 Active Directory 屬性市集，並傳回指定的值  
查詢 Active Directory 必須包含的使用者名稱 \ （的網域 name\) 為最終參數，市集的 Active Directory 屬性可以查詢正確的網域。 否則，支援的同一語法。  
  
下例示範如何查看使用者透過**sAMAccountName**屬性，對方可用自己的網域中，然後再將**郵件**屬性：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer == "AD AUTHORITY"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"), query = "sAMAccountName={0};mail;{1}", param = regexreplace(c.Value, "(?<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
### <a name="example-how-to-query-an-active-directory-attribute-store-based-on-the-value-of-an-incoming-claim"></a>範例： 如何查詢 Active Directory 屬性存放區的價值，連入宣告  
  
```  
c:[Type == "http://test/name"]  
  
   => issue(store = "Enterprise AD Attribute Store",  
  
         types = ("http://test/email"),  
  
         query = ";mail;{0}",  
  
         param = c.Value)  
  
```  
  
下列三個部分是由先前查詢所組成：  
  
-   LDAP 篩選器 — 指定這部分的查詢擷取您想要查詢屬性的物件。 如需有效 LDAP 查詢一般資訊，RFC 2254。 當您正在查詢 Active Directory 屬性網上商店和您不指定 LDAP 篩選 samAccountName\ = {0} 查詢和 Active Directory 屬性市集中所預期的參數，可摘要的值為 {0}。 否則，查詢會導致發生錯誤。 Active Directory 以外 LDAP 屬性網上商店，您無法省略查詢 LDAP 篩選部分或查詢會導致發生錯誤。  
  
-   屬性規格，您可以在此第二個部分查詢，指定屬性 \ （這是如果您使用多個屬性 values\ comma\ 分隔） 您想要退出篩選物件。 屬性，指定數目必須符合您所定義在查詢宣告類型的數目。  
  
-   Active Directory domain-時，才屬性存放區 Active Directory 指定查詢的最後一部分。 \ (當您查詢其他屬性存放區。 並不需要 \) 查詢的這一角用於指定表單 domain\\name 帳號。 Active Directory 屬性網上商店使用網域部分判斷適當的網域控制站連接到執行查詢及要求屬性。  
  
### <a name="example-how-to-use-two-custom-rules-to-extract-the-manager-e-mail-from-an-attribute-in-active-directory"></a>範例： 如何使用自訂的兩規則從 Active Directory 中屬性解壓縮管理員 e\ 郵件  
下列兩個自訂規則，如下所示，順序一起使用時查詢 Active Directory 適用於**管理員]**屬性的使用者考慮 \(Rule 1\)，然後使用該屬性查詢管理員的使用者 account**郵件**屬性 \(Rule 2\)。 最後，**郵件**如 「 ManagerEmail 」 取得發出屬性。 在 [摘要] 規則 1 查詢 Active Directory，並傳送規則 2，然後擷取管理員 e\ 郵件值查詢的結果。  
  
例如，執行這些規則完成，理賠要求會發出包含主管 e\ 郵件的地址 corp.fabrikam.com 網域中的使用者。  
  
**1 規則**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
=> add(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"), query = "sAMAccountName=  
{0};mail,userPrincipalName,extensionAttribute5,manager,department,extensionAttribute2,cn;{1}", param = regexreplace(c.Value, "(?  
<domain>[^\\]+)\\(?<user>.+)", "${user}"), param = c.Value);  
```  
  
**規則 2**  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
&& c1:[Type == "http://schemas.xmlsoap.org/claims/ManagerDistinguishedName"]  
=> issue(store = "Active Directory", types = ("http://schemas.xmlsoap.org/claims/ManagerEmail"), query = "distinguishedName={0};mail;{1}", param = c1.Value,   
param = regexreplace(c1.Value, ".*DC=(?<domain>.+),DC=corp,DC=fabrikam,DC=com", "${domain}\username"));  
```  
  
> [!NOTE]  
> 本規則運作只有在相同的使用者網域中的使用者的管理員是 \ (在本 example\ corp.fabrikam.com)。  
  
## <a name="additional-references"></a>其他參考資料  
[建立為宣告傳送 LDAP 屬性規則](https://technet.microsoft.com/library/dd807115.aspx)  
  

