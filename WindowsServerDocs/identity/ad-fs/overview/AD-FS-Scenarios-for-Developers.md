---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: 適合開發人員使用的 AD FS 案例
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a3156eefc4af52fb7daefb618c689b78fef5efc
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188825"
---
# <a name="ad-fs-scenarios-for-developers"></a>適合開發人員使用的 AD FS 案例


Windows Server 2016 [AD FS 2016] 中的 AD FS 可讓您新增的業界標準的 OpenID Connect 和 OAuth 2.0 型驗證和授權給您正在開發，應用程式，並讓這些應用程式驗證使用者直接針對 AD FS。    
  
AD FS 2016 也支援 WS-同盟、 Ws-trust 和 SAML 通訊協定，以及我們的設定檔已支援在舊版本。  如果您有興趣開發人員指導方針中對於這些通訊協定，請參閱以下的文章。  這篇文章將著重於如何使用及受益於較新的通訊協定支援。  
  
## <a name="why-modern-authentication"></a>為什麼新式驗證  
雖然您可以繼續使用 AD FS 進行登上 WS-同盟、 Ws-trust 和 SAML 通訊協定，正如您有之前，您也可以使用較新的通訊協定，您會獲得下列優點：  
  
* **簡單和一致性**  
    * 若要能夠登入使用相同的一組 Api 和模式：   
        *   多種類型的應用程式 （伺服器、 桌面、 行動、 瀏覽器）  
        *   多個平台 (android、 iOS、 Windows)  
        *   在公司網路內的應用程式或裝載於雲端  
    * 使用同一組程式庫，您已可使用以驗證對 Azure AD 的使用者  
* **彈性**  
    * 除了標準的使用者授權，例如啟用更複雜的案例：      
        * ? 3-legged 登入的使用者授與一個 web 應用程式或服務存取的資源存在於與另一個 web 應用程式或服務的流程。    
        * ? 在的中介層服務存取後端 API 的伺服器對伺服器流程  
        * ? JavaScript 架構的單一頁面應用程式 (SPA)  
* **業界支援**  
    * OAuth 2.0 和 OpenID Connect 享受寬使用率跨產業，因此這些模式的知識可協助您啟用驗證和授權的 Active Directory 環境之外  
  
## <a name="how-it-works-the-basics"></a>它的運作方式：基本概念  
您可以將 AD FS 的新式驗證加入您的應用程式使用同一組工具和程式庫，您已可使用以驗證對 Azure AD 的使用者。   
  
在 AD FS 案例當然，它是 AD FS 和沒有 Azure AD 作為身分識別提供者和授權伺服器。  否則概念都完全相同： 使用者提供其認證，並取得權杖，直接或透過媒介，以存取資源。  
  
最基本的案例是由使用者或 「 資源擁有者 」，互動的瀏覽器來存取 web 應用程式所組成：  
  
![適用於開發人員的 AD FS](media/ADFS_DEV_1.png)  
  
Web 應用程式稱為 「 用戶端 」，因為它的資源的存取權杖啟動授權伺服器 (AD FS) 的要求。  資源可能會由 web 應用程式本身，或可能與網路或網際網路上的某個位置的 web API 存取。   使用者或 「 資源擁有者 」 授權接收該存取權杖，藉由提供對授權伺服器的認證將用戶端 web 應用程式。    
  
## <a name="how-it-works-components"></a>它的運作方式： 元件  
OAuth 2.0 和 OpenID Connect 案例中進行 AD FS 使用同一組工具和 Azure AD 是身分識別提供者時，您使用的程式庫。  這些元件包括：  
* Active Directory Authentication Library (ADAL): 用戶端程式庫，協助收集使用者認證、 建立和提交權杖要求以及擷取所產生的權杖。    
* (Open Web Interface for.NET) 的 OWIN 中介軟體：雖然 OWIN 為基礎的社群專案，Microsoft 已建立一組伺服器端程式庫，來保護 web 應用程式和 web Api 使用 OpenID Connect 和 OAuth 2.0  
  
下圖顯示這些元件的角色：  
  
![適用於開發人員的 AD FS](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>在 AD FS 2016 中模型化這些案例  
  
### <a name="application-groups"></a>應用程式群組  
若要表示 AD FS 原則中的這些案例，我們引進了稱為應用程式群組的新概念。  應用程式群組可包含任意數目和應用程式的下列基本類型的組合：  
  
  
  
應用程式群組 / 應用程式類型  |描述  |[角色]    
---------|---------|---------  
原生應用程式     |  有時也稱為公用用戶端，這被為了要執行於電腦或裝置與使用者互動的用戶端應用程式。       | 要求權杖向授權伺服器 (AD FS) 的使用者存取資源。  將 HTTP 要求傳送至受保護的資源，將權杖當做 HTTP 標頭中。        
伺服器應用程式     |   Web 應用程式的伺服器上執行，並透過瀏覽器的使用者通常可以存取。  因為它能夠維護其自己的用戶端 'secret' 或認證，有時稱為機密用戶端。      | 要求權杖向授權伺服器 (AD FS) 的使用者存取資源。  將 HTTP 要求傳送至受保護的資源，將權杖當做 HTTP 標頭中。               
Web API     |  結束資源使用者存取。 想像這是 「 信賴憑證者的合作對象 」 的新表示法。| 使用用戶端所取得的權杖  
  
### <a name="differences-from-ad-fs-2012-r2"></a>從 AD FS 2012 R2 的差異  
應用程式群組會將 AD FS 2012 R2 分別公開為信賴憑證者的合作對象、 用戶端和應用程式權限的信任和授權的項目。  
  
下表會比較所對應的應用程式信任物件會在 AD FS 2012 R2 中與 AD FS 2016 的方法：  
  
Windows Server 2012 R2 的 AD FS|在 PowerShell 中|AD FS 管理  
---------|---------|---------  
新增原生用戶端|Add-AdfsClient|NA  
將伺服器應用程式新增為用戶端|Add-AdfsClient|NA  
新增 Web API / 資源|Add-AdfsRelyingPartyTrust|建立信賴憑證者信任  
  
AD FS 2016|在 PowerShell 中|AD FS 管理  
---------|---------|---------  
新增原生用戶端|Add-AdfsNativeClientApplication|新增原生應用程式群組  
將伺服器應用程式新增為用戶端|Add-AdfsServerApplication|新增伺服器應用程式群組  
新增 Web API / 資源|Add-AdfsWebApiApplication|新增 Web API 應用程式群組  
  
### <a name="application-permissions-and-consent"></a>應用程式權限及同意  
根據預設，應用程式群組中的用戶端可以存取相同的群組中的資源。  若要設定特定的應用程式權限沒有系統管理員。  應用程式群組也可讓系統管理員指定允許，例如 openid 或 user_impersonation 範圍。  以下案例說明指定到底是哪些案例需要的範圍。  
  
由於 AD FS 使用的系統管理員的同意模型，不會提示使用者同意存取資源時。  藉由設定應用程式群組，系統管理員實際上會提供代表應用程式的所有使用者的同意。  
  
## <a name="supported-scenarios"></a>支援的案例  
下一節會說明我們支援更多詳細資料中的案例。  
  
### <a name="tokens-used"></a>使用 token  
這些案例會使用三個語彙基元的型別：  
  
* **id_token:** JWT 權杖，用來代表使用者的身分識別。 Id_token 的 'aud' 或對象宣告比對原生模式或伺服器應用程式的用戶端識別碼。  
* **access_token:** 使用 JWT 權杖中的 Oauth 和 OpenID connect 案例和可供資源的主要。  此權杖的 'aud' 或對象宣告必須符合資源或 Web API 的識別碼。  
* **refresh_token:** 提交此權杖來取代收集使用者認證，以提供單一登入體驗。  這個語彙基元已發出並使用 AD FS，而且無法讀取用戶端或資源。    
  
### <a name="native-client-to-web-api"></a>對 Web API 的原生用戶端  
此案例可讓呼叫的 AD FS 2016 的受保護的 Web API 的原生用戶端應用程式的使用者。  
* 原生用戶端應用程式使用 ADAL 來傳送授權和權杖要求，以提示輸入認證時如有必要，使用者的 AD FS，然後傳送至 Web API 要求的 HTTP 標頭為產生的權杖  
* [僅針對示範目的為此組件]Web API 會讀取結果從用戶端，傳送存取權杖，並將其傳送回用戶端的 ClaimsPrincipal 物件中的宣告。  
  
![通訊協定流程的描述](media/ADFS_DEV_3.png)  
  
1.  原生用戶端應用程式起始的 ADAL 程式庫呼叫流程。  這會觸發瀏覽器為基礎 HTTP GET 至 AD FS 授權端點：  
  
**授權要求：**  
取得 https://fs.contoso.com/adfs/oauth2/authorize?  
  
參數|值  
---------|---------  
response_type|「 程式碼 」  
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）  
client_id|用戶端應用程式群組中的原生應用程式識別碼  
redirect_uri|重新導向 URI 的應用程式群組中的原生應用程式  
  
**授權要求的回應：**  
如果使用者未登入之前，會提示使用者輸入認證。    
AD FS 會透過做為 redirect_uri 的查詢元件中的"code"參數傳回授權碼回應。  例如: HTTP/1.1 302 已找到位置：  **http://redirect_uri:80/?code=&lt; 程式碼&gt;。**  
  
2.  然後，原生用戶端會傳送的程式碼，以及下列參數，對 AD FS 權杖端點：  
  
**權杖的要求：**  
貼文 https://fs.contoso.com/adfs/oauth2/token  
  
參數|值  
---------|---------  
grant_type|"authorization_code" 
code|從 1 的授權碼  
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）  
client_id|用戶端應用程式群組中的原生應用程式識別碼  
redirect_uri|重新導向 URI 的應用程式群組中的原生應用程式  
  
**權杖要求的回應：**  
AD FS 會使用 access_token、 refresh_token 和在本文中的 id_token 以 HTTP 200 回應。  
  
3.  原生應用程式接著會將上述回應的 access_token 部分做授權標頭在 HTTP 要求傳送至 web API。  
  
### <a name="single-sign-on-behavior"></a>單一登入行為  
後續的用戶端要求中，1 小時 （依預設） access_token 仍然會有效快取中，而且新的要求將不會觸發任何 AD fs 的流量。  自動將 adal 從快取擷取的 access_token。  
  
存取權杖過期後，ADAL 會自動將重新整理權杖型的要求傳送至 AD FS 權杖端點 （略過授權要求自動）。  
**重新整理權杖要求：**  
貼文 https://fs.contoso.com/adfs/oauth2/token
   

參數|值|
---------|---------
grant_type|"refresh_token"|
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）|
client_id|用戶端應用程式群組中的原生應用程式識別碼
refresh_token|在初始權杖要求的回應中的 AD FS 所發出的重新整理權杖

  
  
**重新整理權杖要求的回應：**  
如果在 < SSO_period > 內的重新整理權杖，則要求會導致新的存取權杖。 使用者不會提示輸入認證。  如需有關 SSO 設定，請參閱[AD FS 單一登入設定](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
要求重新整理權杖是否過期，導致的 HTTP 401 錯誤"invalid_grant"，"error_description"與"MSIS9615:Refresh_token 參數中所收到的重新整理權杖已過期 」。 在此情況下，ADAL 會自動提交新的授權要求看起來就像上述 #1。    
  
### <a name="web-browser-to-web-app"></a>網頁瀏覽器到 Web 應用程式   
在此案例中，使用瀏覽器使用者需要存取裝載的 web 應用程式的資源。    
有兩個案例可以達成這個目的。  
  
#### <a name="oauth-confidential-client"></a>Oauth 機密用戶端  
此案例的類似於上述程式碼中的授權要求，後面接著權杖交換的程式碼。  Web 應用程式 （模型化為伺服器應用程式在 AD FS） 起始授權要求，透過瀏覽器，並交換權杖的程式碼 （透過直接連線到 AD FS）  
  
![通訊協定流程的描述](media/ADFS_DEV_4.png)  
  
1.  授權要求中透過瀏覽器，會將傳送 HTTP GET 至 AD FS Web 應用程式起始授權端點  
**授權要求**:  
取得 https://fs.contoso.com/adfs/oauth2/authorize?  
  
參數|值  
---------|---------  
response_type|「 程式碼 」  
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）  
client_id|用戶端應用程式群組中的原生應用程式識別碼  
redirect_uri|重新導向 URI 的應用程式群組中的 web 應用程式 （伺服器應用程式）  
  
授權要求的回應：  
如果使用者未登入之前，會提示使用者輸入認證。  
AD FS 會藉由例如做為 redirect_uri，查詢元件中的"code"參數傳回授權碼回應：HTTP/1.1 302 已找到位置： https://webapp.contoso.com/?code=&lt; 程式碼&gt;。  
  
2.  由於上述的 302，瀏覽器會起始 HTTP GET 至 web 應用程式，例如：取得 http://redirect_uri:80/?code=&lt; 程式碼&gt;。   
  
3.  Web 應用程式，接收過程式碼，此時會起始對 AD FS 的權杖端點，傳送下列要求  
**權杖的要求：**  
貼文 https://fs.contoso.com/adfs/oauth2/token  
  
參數|值  
---------|---------  
grant_type|"authorization_code"  
code|從上述的 2 的授權碼  
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）  
client_id|應用程式群組中的 web 應用程式 （伺服器應用程式） 的用戶端識別碼  
redirect_uri|重新導向 URI 的應用程式群組中的 web 應用程式 （伺服器應用程式）  
client_secret|應用程式群組中的 web 應用程式 （伺服器應用程式） 的密碼。 **注意：用戶端的認證不需要是 client_secret。AD FS 支援以及使用憑證或 Windows 整合式驗證的能力。**  
  
**權杖要求的回應：**  
AD FS 會使用 access_token、 refresh_token 和在本文中的 id_token 以 HTTP 200 回應。  
宣告  
4.  Web 應用程式則可能會耗用 access_token 上述之回應的一部分 （在 web 應用程式本身在其中裝載資源案例），否則為將它傳送為授權標頭在 HTTP 要求到 web API。  
  
#### <a name="single-sign-on-behavior"></a>單一登入行為  
雖然存取權杖仍然有效 （1 小時） （依預設） 用戶端的快取中，您可能會認為，第二個要求能與上述-原生用戶端案例相同，新的要求不會觸發對 AD FS 的任何流量的存取權杖將會自動為會從快取擷取的 ADAL。  不過，就可以在 web 應用程式可以傳送不同的授權和權杖要求，其前身，透過不同的 URL 連結，如範例所示。  
  
在此情況下，它是可讓發出新的授權碼，而不提示使用者輸入認證的 AD FS 的 AD FS 瀏覽器 SSO cookie。 Web 應用程式接著會呼叫至 AD FS 來交換新的授權碼，做為新的存取權杖。  使用者不會提示輸入認證。  
  
否則如果 web 應用程式智慧足以知道是否使用者尚未通過驗證，可以略過授權要求和任一個：  
* 快取的存取權杖，如果尚未過期，擷取和使用，或   
* 要求的權杖型的要求可傳送至 AD FS 權杖端點，如下所述  
  
**重新整理權杖要求：**  
貼文 https://fs.contoso.com/adfs/oauth2/token
   
參數|值  
---------|---------  
grant_type|"refresh_token"  
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）  
client_id|應用程式群組中的 web 應用程式 （伺服器應用程式） 的用戶端識別碼  
refresh_token|重新整理的初始權杖要求的回應中的 AD FS 所發出的權杖  
client_secret|Web 應用程式 （伺服器應用程式），應用程式群組中的祕密  
  
**重新整理權杖要求的回應：**  
如果在 < SSO_period > 內的重新整理權杖，則要求會導致新的存取權杖。 使用者不會提示輸入認證。 如需有關 SSO 設定，請參閱[AD FS 單一登入設定](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
要求重新整理權杖是否過期，導致的 HTTP 401 錯誤"invalid_grant"，"error_description"與"MSIS9615:Refresh_token 參數中所收到的重新整理權杖已過期 」。 在此情況下，ADAL 會自動提交新的授權要求看起來就像上述 #1。    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID Connect:混合式流程  
此案例的類似於上述程式碼中，有的授權要求起始的 web 應用程式，透過瀏覽器重新導向和 AD fs 的權杖交換 web 應用程式的程式碼。  在此案例中的差異是 AD FS 簽發 id_token，做為初始授權要求回應的一部分。  
  
![通訊協定流程的描述](media/ADFS_DEV_5.png)  
  
1.  授權要求中透過瀏覽器，會將傳送 HTTP GET 至 AD FS Web 應用程式起始授權端點  
  
**授權要求：**  
取得 https://fs.contoso.com/adfs/oauth2/authorize?  
  
參數|值  
---------|---------  
response_type|"code+id_token"  
response_mode|"form_post"  
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）  
client_id|應用程式群組中的 web 應用程式 （伺服器應用程式） 的用戶端識別碼  
redirect_uri|重新導向 URI 的應用程式群組中的 web 應用程式 （伺服器應用程式）  
  
**授權要求的回應：**  
如果使用者未登入之前，會提示使用者輸入認證。  
AD FS 會以 HTTP 200 和表單，其中包含以下的 做為隱藏項目：  
* 程式碼： 授權碼  
* id_token: JWT 權杖中包含描述使用者驗證的宣告  
2.  表單會自動張貼到 web 應用程式，傳送至 web 應用程式的程式碼，並在 id_token 的 redirect_uri。  
  
3.  Web 應用程式，接收過程式碼，此時會起始對 AD FS 的權杖端點，傳送下列要求  
  
**權杖的要求：**  
貼文 https://fs.contoso.com/adfs/oauth2/token
  
  
  
參數|值  
---------|---------  
grant_type|"authorization_code"  
code|從上方的授權碼  
resource|Web API 應用程式群組中的 RP 識別碼 （識別碼）  
client_id|應用程式群組中的 web 應用程式 （伺服器應用程式） 的用戶端識別碼  
redirect_uri|重新導向 URI 的應用程式群組中的 web 應用程式 （伺服器應用程式）  
client_secret|Web 應用程式 （伺服器應用程式），應用程式群組中的祕密  
  
**權杖要求的回應：**  
AD FS 會使用 access_token、 refresh_token 和在本文中的 id_token 以 HTTP 200 回應。  
  
4.  Web 應用程式則可能會耗用 access_token 上述之回應的一部分 （在 web 應用程式本身在其中裝載資源案例），否則為將它傳送為授權標頭在 HTTP 要求到 web API。  
  
#### <a name="single-sign-on-behavior"></a>單一登入行為  
單一登入行為會與上述的 Oauth 2.0 機密用戶端流程相同。  
  
### <a name="on-behalf-of"></a>代表  
在此案例中，web 應用程式會使用使用者的原始存取權杖來要求並取得另一個 web 應用程式再將與終端使用者存取的 Web API 中的另一個存取權杖。  這稱為 「 代理者 」 流程。  
  
![通訊協定流程的描述](media/ADFS_DEV_6.png)  
  
如同步驟 3 和 4，在先前的流程中的步驟 1 和 2 工作。  
步驟 3 中，關鍵需求是 client_id 參數，而 Web 應用程式 2 的用戶端識別碼必須符合 RP 識別碼的 Web API a。 換句話說，在新語彙基元交換存取權杖的對象必須符合要求新權杖之實體的用戶端識別碼。  

## <a name="related-content"></a>相關內容  
請參閱[AD FS 開發](../AD-FS-Development.md)的逐步解說文章的完整清單，提供逐步指示需使用相關的流程。 
