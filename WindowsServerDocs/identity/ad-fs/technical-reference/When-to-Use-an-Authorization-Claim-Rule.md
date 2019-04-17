---
ms.assetid: b734cbcb-342c-4a28-8ab5-b9cd990bb1c2
title: "使用授權理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 144e382692e8f2a6732f8c7c5b8a1dd6049192cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-an-authorization-claim-rule"></a>使用授權理賠要求規則
當您需要輸入宣告類型並再適用於執行的動作，會判斷使用者是否會允許或無法存取您指定規則值，您可以使用在 Active Directory 同盟服務 \(AD FS\) 本規則。 當您使用此規則時，您通過或轉換宣告符合下列規則邏輯操作，根據您設定的規則選項：  
  
|規則選項|邏輯規則|  
|---------------|--------------|  
|允許所有使用者|如果輸入宣告類型等於*任何宣告類型*和值等*的任何值*，問題然後取得的值等*允許*|  
|允許此傳入理賠要求的使用者存取|如果輸入宣告類型等於*指定宣告類型*和值等*指定宣告值*，問題然後取得的值等*允許*|  
|拒絕這個傳入理賠要求的使用者存取|如果輸入宣告類型等於*指定宣告類型*和值等*指定宣告值*，問題然後取得的值等*拒絕*|  
  
下列章節提供基本簡介取得規則，並提供時使用此規則有關的進一步詳細資料。  
  
## <a name="about-claim-rules"></a>關於理賠要求規則  
宣告規則表示商務邏輯操作，將需要連入宣告、 適用於條件的執行個體 \ （如果 x 然後 y\） 和產生傳出宣告依據條件的參數。 下列清單輪廓重要您進一步讀取之前，您必須知道的相關的提示取得規則本主題中：  
  
-   AD FS snap\ 中管理，在理賠要求規則只能建立使用理賠要求規則範本  
  
-   宣告規則處理程序傳入宣告直接從宣告提供者 \ （例如 Active Directory 或其他聯盟 Service\） 或接受的輸出從轉換宣告提供者信任規則。  
  
-   宣告發行引擎順序特定的規則集中處理理賠要求規則。 藉由設定優先順序規則，可以進一步改善或篩選宣告專特定的規則設定中的上一個規則。  
  
-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則相同宣告類型處理多個理賠要求值。  
  
如需詳細資訊理賠要求規則及宣告規則集合，請查看[的角色的取得規則](The-Role-of-Claim-Rules.md)。 如需規則的處理方式的相關資訊，請查看[的角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。 宣告規則集合的處理方式的相關資訊，請查看[的角色宣告管線的](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="permit-all-users"></a>允許所有使用者  
當您使用 [允許所有使用者規則範本時，所有使用者將都可以存取信賴。 不過，您可以使用其他授權規則進一步的限制存取。 如果一個規則允許使用者存取信賴，另一個規則的使用者存取拒絕信賴，拒絕將會覆寫允許結果，使用者無法存取。  
  
使用者可以存取信賴從同盟服務可能仍然無法服務所信賴。  
  
## <a name="permit-access-to-users-with-this-incoming-claim"></a>允許此傳入理賠要求的使用者存取  
當您使用允許] 或 [拒絕型使用者在收到取得規則範本建立規則，並設定的條件，以允許時，您可以存取特定使用者的許可信賴根據類型及連入宣告的值。 例如，您可以使用此規則範本來建立，允許的值為網域系統管理員取得群組那些使用者規則。 如果一個規則允許使用者存取信賴，另一個規則的使用者存取拒絕信賴，拒絕將會覆寫允許結果，使用者無法存取。  
  
使用者可以存取信賴從同盟服務可能仍然無法服務信賴。 如果您想要允許所有使用者存取信賴，使用都允許所有使用者規則範本。  
  
## <a name="deny-access-to-users-with-this-incoming-claim"></a>拒絕這個傳入理賠要求的使用者存取  
當您使用允許] 或 [拒絕型使用者在收到取得規則範本建立規則，並設定拒絕條件時，您可以根據類型及值連入宣告信賴拒絕使用者的存取權。 例如，您可以使用此規則範本建立會拒絕群組的所有使用者都取得值網域使用者的使用規則。  
  
如果您想要使用的拒絕條件，但也可讓存取特定使用者信賴，您必須稍後明確新增與允許條件以信賴那些使用者存取授權規則。  
  
如果使用者無法存取宣告發行引擎處理規則設定時，進一步規則處理關機時，並在 AD FS 傳回 「 存取 「 錯誤的使用者要求。  
  
## <a name="authorizing-users"></a>授權使用者  
AD FS 中, 授權規則用於發行允許或拒絕理賠要求的使用者或群組中的使用者是否會判斷 \ （根據理賠要求輸入 used\) 會允許或無法存取 Web\ 型指定信賴的資源。 授權規則只能信賴廠商信任上設定。  
  
### <a name="authorization-rule-sets"></a>授權規則集合  
其他授權規則集存在根據允許的類型或拒絕您需要進行的操作。 這些規則集包括：  
  
-   **發行授權規則**： 本規則判斷使用者是否可以收到宣告信賴的並，因此可存取信賴。  
  
-   **委派授權規則**： 本規則判斷使用者是否可做為另一位使用者信賴。 當使用者做為另一位使用者時，宣告要求的使用者仍然位於預付碼。  
  
-   **模擬授權規則**： 本規則判斷使用者是否可以完全模擬信賴的其他使用者。 模擬另一位使用者，所以功能非常強大信賴並不知道正在模擬使用者。  
  
如需詳細資訊授權規則程序如何納入宣告發行管線，查看宣告發行引擎的角色。  
  
### <a name="supported-claim-types"></a>支援的宣告類型  
廣告 FSdefines 兩個宣告用來判斷使用者是否要允許或拒的類型。 這些取得類型統一資源識別碼 \(URIs\) 如下所示：  
  
1.  **允許**: http:///\/schemas.microsoft.com\/authorization\/claims\/permit  
  
2.  **拒絕**: http:///\/schemas.microsoft.com\/authorization\/claims\/deny  
  
## <a name="how-to-create-this-rule"></a>如何建立本規則  
您可以使用理賠要求規則語言或使用兩個授權規則來建立**允許所有使用者**規則範本或**允許] 或 [拒絕使用者根據傳入取得**AD FS 管理 snap\ 中的 [規則範本。 允許所有使用者規則範本不提供任何設定的選項。 不過，允許] 或 [拒絕使用者根據連入宣告規則範本提供下列設定選項：  
  
-   指定名稱理賠要求規則  
  
-   指定傳入宣告類型  
  
-   輸入傳入宣告值  
  
-   允許此傳入理賠要求的使用者存取  
  
-   拒絕這個傳入理賠要求的使用者存取  
  
如需如何建立此範本後續的指示操作，[建立規則允許所有使用者](https://technet.microsoft.com/library/ee913577.aspx)或[上連入宣告建立規則允許或拒絕型使用者](https://technet.microsoft.com/library/ee913594.aspx)中的 AD FS 部署。  
  
## <a name="using-the-claim-rule-language"></a>使用語言理賠要求規則  
如果宣告值符合自訂模式時，才應傳送理賠要求，您必須使用 [自訂規則。 如需詳細資訊，請查看[使用自訂理賠要求規則](When-to-Use-a-Custom-Claim-Rule.md)。  
  
### <a name="example-of-how-to-create-an-authorization-rule-based-on-multiple-claims"></a>如何建立根據多宣告授權規則的範例  
當使用理賠要求規則語言語法授權宣告，請理賠要求可以也會發出根據有多個索賠項目中的使用者原始宣告。 下列規則問題授權宣告使用者編輯器群組成員，並使用 Windows 驗證驗證時才：  
  
```  
[type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod",   
value == "urn:federation:authentication:windows" ]  
&& [type == "http://schemas.xmlsoap.org/claims/Group ", value == “editors”]   
=> issue(type = "http://schemas.xmlsoap.org/claims/authZ", value = "Granted");  
```  
  
### <a name="example-of-how-to-create-authorization-rules-that-will-delegate-who-can-create-or-remove-federation-server-proxy-trusts"></a>如何建立授權的範例規則，將會委派人員可以建立，或移除聯盟伺服器 proxy 信任  
聯盟服務可以使用聯盟 proxy 伺服器重新導向 client 要求之前，同盟服務與聯盟伺服器 proxy 電腦之間必須先建立信任。 根據預設，建立 proxy 信任時其中一項下列認證提供成功 AD FS 聯盟伺服器 Proxy 設定精靈中：  
  
-   服務帳號，並使用的 proxy 將保護同盟服務，  
  
-   Active Directory domain account 上所有的聯盟伺服器聯盟伺服器在本機系統管理員群組成員  
  
當您想要指定的使用者可以建立 proxy 信任指定同盟服務時，您可以使用下列委派方法。 此清單的方法是根據最安全且至少問題的方法委派 AD FS product 小組建議的優先順序。 它會需要使用只是一種方法，根據您的組織的需求：  
  
1.  在 Active Directory 中建立網域安全性群組 \ （例如，FSProxyTrustCreators\），將這個群組新增至本機系統管理員群組每個聯盟伺服器，並再新增只使用者帳號，您要委派新群組此權限。 這是慣用的方法。  
  
2.  新增使用者的網域 account 每個聯盟伺服器管理員群組。  
  
3.  如果因為某些原因您無法使用這兩種方法，您也可以為這個項目的建立授權規則。 雖然不建議這樣做，因為可能複雜是否有此規則撰寫不正確，可能會發生的您可以使用 [自訂授權規則委派網域帳號可以也建立，或甚至移除之間指定同盟服務相關聯的所有聯盟伺服器 proxy 信任的 Active Directory。  
  
    如果您選擇方法 3 時，您可以使用下列語法規則發行，可讓指定的使用者的授權宣告 \ （在本案例，contoso\\frankm\） 來建立信任一或多個聯盟 proxy 伺服器同盟服務。 您必須套用此規則使用 Windows PowerShell 命令**Set\-ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", issuer=~"^AD AUTHORITY$" value == "contoso\frankm" ] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true")  
  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
    之後，如果您想要移除，讓使用者可以不再建立 proxy 信任的使用者，您可以回復至預設 proxy 信任的授權来移除的規則使用者建立 proxy 信任同盟服務的權限。 您必須也適用於此規則使用 Windows PowerShell 命令**Set\-ADFSProperties AddProxyAuthorizationRules**。  
  
    ```  
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-544", Issuer =~ "^AD AUTHORITY$"])   
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^AD AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustManagerSid({0})", param= c.Value );  
  
    c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/proxytrustid", Issuer =~ "^SELF AUTHORITY$" ] => issue(store="_ProxyCredentialStore",types=("https://schemas.microsoft.com/authorization/claims/permit"),query="isProxyTrustProvisioned({0})", param=c.Value );  
    ```  
  
如需有關如何使用理賠要求規則語言，請查看[角色取得規則語言的](The-Role-of-the-Claim-Rule-Language.md)。  
  

