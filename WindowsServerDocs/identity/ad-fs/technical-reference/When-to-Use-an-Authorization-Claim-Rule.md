---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: 使用授權宣告規則的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6b852a580bdc0ea02643d478dc51b5cbcd2eac4b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188315"
---
# <a name="when-to-use-an-authorization-claim-rule"></a>使用授權宣告規則的時機
您可以使用這項規則中 Active Directory Federation Services \(AD FS\)當您需要將傳入的宣告型別，然後再套用動作，藉此決定是否將允許或拒絕存取使用者為基礎的值，您在規則中指定。 當您使用此規則時，您會根據您在規則中設定的選項之一，傳遞或轉換符合下列規則邏輯的宣告。  
  
|規則選項|規則邏輯|  
|---------------|--------------|  
|允許所有使用者|如果傳入宣告類型等於「任何宣告類型」  且值等於「任何值」  ，則會發行值等於「允許」 |  
|允許具有這個傳入宣告的使用者存取|如果傳入宣告類型等於「指定的宣告類型」  且值等於「指定的宣告值」  ，則會發行值等於「允許」  的宣告|  
|拒絕具有這個傳入宣告的使用者存取|如果傳入宣告類型等於「指定的宣告類型」  且值等於「指定的宣告值」  ，則會發行值等於「拒絕」 |  
  
下列章節提供宣告規則的基本介紹，並進一步提供如何使用此規則的詳細資訊。  
  
## <a name="about-claim-rules"></a>關於宣告規則  
宣告規則表示商務邏輯，會收取傳入宣告，對其套用條件的執行個體\(若 x 則 y\)和產生根據條件參數傳出宣告。 在您進一步閱讀本主題之前，請先閱讀下列清單，其中會概述關於宣告規則您應該知道的重要秘訣：  
  
-   在 AD FS 管理嵌入式管理單元\-，宣告規則只能建立使用宣告規則範本  
  
-   宣告規則處理程序內送宣告直接從宣告提供者\(Active Directory 或其他同盟服務等\)或宣告提供者信任上從輸出的接受轉換規則。  
  
-   宣告規則的處理方式，是由宣告發行引擎依時間先後順序按照指定的規則集處理。 藉由設定規則優先順序，您可以進一步精簡或篩選由特定規則集內的上一個規則所產生的宣告。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則和同一宣告類型處理多個宣告值。  
  
如需詳細的宣告規則和宣告規則集的相關資訊，請參閱[規則的角色宣告](The-Role-of-Claim-Rules.md)。 如需有關規則的處理方式的詳細資訊，請參閱 < [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 宣告規則集的處理方式的詳細資訊，請參閱[The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="permit-all-users"></a>允許所有使用者  
當您使用「允許所有使用者」規則範本時，所有的使用者都將可存取信賴憑證者。 不過，您可以使用其他授權規則進一步限制存取權。 如果一項規則允許使用者存取信賴憑證者，而另一個規則拒絕使用者存取信賴憑證者，拒絕結果將會覆寫允許結果，而拒絕使用者的存取。  
  
從同盟服務獲准存取信賴憑證者的使用者，仍可能遭信賴憑證者拒絕服務。  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>允許具有這個傳入宣告的使用者存取  
當您使用「根據傳入宣告允許或拒絕使用者」規則範本來建立規則和設定允許的條件時，您可以根據傳入宣告的類型和值，允許特定使用者存取信賴憑證者。 例如，您可以使用此規則範本建立下列規則：僅允許群組宣告值為 Domain Admins 的使用者。 如果一項規則允許使用者存取信賴憑證者，而另一個規則拒絕使用者存取信賴憑證者，拒絕結果將會覆寫允許結果，而拒絕使用者的存取。  
  
從同盟服務獲准存取信賴憑證者的使用者，仍可能遭信賴憑證者拒絕服務。 如果您想要允許所有使用者存取信賴憑證者，請使用「允許所有使用者」規則範本。  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>拒絕具有這個傳入宣告的使用者存取  
當您使用「根據傳入宣告允許或拒絕使用者」規則範本來建立規則和設定拒絕的條件時，您可以根據傳入宣告的類型和值，拒絕使用者存取信賴憑證者。 例如，您可以使用此規則範本建立下列規則：拒絕群組宣告值為 Domain Users 的所有使用者。  
  
如果您想要使用拒絕條件，但同時允許特定使用者存取信賴憑證者，您後續必須明確地新增授權規則，以及讓這些使用者存取信賴憑證者的允許條件。  
  
如果使用者被拒絕存取，當宣告發行引擎處理規則集時，處理其他規則關閉時，與 AD FS 會傳回 「 拒絕存取 」 錯誤，使用者的要求。  
  
## <a name="authorizing-users"></a>授權使用者  
在 AD FS 中，授權規則用來發出許可或拒絕宣告，以決定使用者或一群使用者\(視使用的宣告類型而定\)允許存取 Web\-架構中指定的信賴憑證者的資源合作對象與否。 只可以在信賴憑證者信任上設定授權規則。  
  
### <a name="authorization-rule-sets"></a>授權規則集  
視您需要設定的允許或拒絕作業類型之不同，會有不同的授權規則集。 這些規則集包括：  
  
-   **發行授權規則**：這些規則會決定使用者是否可以接收信賴憑證者的宣告，並且因此可存取信賴憑證者。  
  
-   **委派授權規則**：這些規則會決定使用者是否可做為信賴憑證者的另一個使用者。 當使用者做為另一位使用者時，要求端使用者的相關宣告仍會放置在權杖中。  
  
-   **模擬授權規則**：這些規則會決定使用者是否可完全模擬信賴憑證者的另一個使用者。 模擬另一個使用者是非常強大的功能，因為信賴憑證者並不知道使用者正被模擬。  
  
如需有關如何將授權規則程序融入宣告發行管線中的詳細資訊，請參閱「宣告發行引擎的角色」。  
  
### <a name="supported-claim-types"></a>支援的宣告類型  
AD FS 會定義兩個用來判斷允許或拒絕使用者的宣告類型。 這些宣告類型的統一資源識別元\(Uri\)如下所示：  
  
1.  **允許**: http:\/\/schemas.microsoft.com\/授權\/宣告\/允許  
  
2.  **拒絕**: http:\/\/schemas.microsoft.com\/授權\/宣告\/拒絕  
  
## <a name="how-to-create-this-rule"></a>如何建立此規則  
您可以建立使用宣告規則語言，或使用這兩個授權規則**允許所有使用者**規則範本或有**允許或拒絕使用者根據傳入宣告**的 AD FS 中的規則範本管理嵌入式管理單元\-中。 「允許所有使用者」規則範本未提供任何設定選項。 但「根據傳入宣告允許或拒絕使用者」規則範本則提供下列設定選項：  
  
-   指定宣告規則名稱  
  
-   指定傳入宣告類型  
  
-   輸入傳入宣告值  
  
-   允許具有這個傳入宣告的使用者存取  
  
-   拒絕具有這個傳入宣告的使用者存取  
  
如需有關如何建立此範本的詳細資訊，請參閱[建立規則以允許所有使用者](https://technet.microsoft.com/library/ee913577.aspx)或是[連入宣告上建立規則以允許或拒絕使用者基礎](https://technet.microsoft.com/library/ee913594.aspx)AD FS 部署指南中。  
  
## <a name="using-the-claim-rule-language"></a>使用宣告規則語言  
如果只有在宣告值符合自訂模式時才應傳送宣告，您必須使用自訂規則。 如需詳細資訊，請參閱 [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md)。  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>說明如何根據多個宣告建立授權規則的範例  
使用宣告規則語言語法來授權宣告時，也可以根據使用者原始宣告中存在的多個宣告來發出宣告。 只有在使用者屬於「編輯者」群組的成員，並且已使用 Windows 驗證通過驗證後，下列規則才會發出授權宣告：  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>說明如何建立授權規則以委派可建立或移除同盟伺服器 Proxy 信任之人員的範例  
必須先建立同盟服務與同盟伺服器 Proxy 電腦之間的信任，同盟服務才可以使用同盟伺服器 Proxy 重新導向用戶端要求。 根據預設，順利在 AD FS 同盟伺服器 Proxy 設定精靈中提供下列其中一個認證時，將會建立 Proxy 信任：  
  
-   Proxy 所將保護的服務帳戶 (由同盟服務使用)  
  
-   在同盟伺服器陣列中的所有同盟伺服器上，屬於本機系統管理員群組成員的 Active Directory 網域帳戶  
  
當您想要指定哪些使用者可以建立給定同盟服務的 Proxy 信任時，您可以使用下列任何委派方法。 此方法清單的優先順序，將取決於 AD FS 產品小組在考量哪個委派方法最安全、問題最少之後所做出的建議。 您只需要根據組織的需求使用其中一個方法即可：  
  
1.  Active Directory 中建立網域安全性群組\(例如 FSProxyTrustCreators\)，將此群組新增至每個伺服陣列中的同盟伺服器上本機 Administrators 群組，然後新增 只在您想要的使用者帳戶若要委派到新的群組此權限。 這是慣用方法。  
  
2.  將使用者的網域帳戶新增至伺服器陣列中每個同盟伺服器的系統管理員群組。  
  
3.  如果您因故無法使用其中一種方法，您也可以建立符合此用途的授權規則。 您可以使用自訂授權規則，指定哪些 Active Directory 網域使用者帳戶也可以建立、甚至移除與指定的同盟服務相關聯的所有同盟伺服器 Proxy 之間的信任。但我們不建議此做法，因為此規則若未正確撰寫，可能會導致複雜的狀況。  
  
    如果您選擇方法 3，您可以使用下列規則語法發出授權宣告，讓指定的使用者\(在此案例中，contoso\\frankm\)建立信任的一或多個同盟伺服器 proxyFederation Service。 您必須套用此規則使用 Windows PowerShell 命令**設定\-ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    如果您後續想要移除使用者，讓該使用者無法再建立 Proxy 信任，您可以還原至預設 Proxy 信任授權規則，以移除使用者建立 Proxy 對同盟服務之信任的權限。 您也必須套用此規則使用 Windows PowerShell 命令**設定\-ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
如需如何使用宣告規則語言的詳細資訊，請參閱[The Role of 宣告規則語言](The-Role-of-the-Claim-Rule-Language.md)。  
  

