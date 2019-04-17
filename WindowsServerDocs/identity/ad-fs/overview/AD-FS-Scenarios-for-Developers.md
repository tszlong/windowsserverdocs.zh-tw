---
ms.assetid: 8a64545b-16bd-4c13-a664-cdf4c6ff6ea0
title: "適用於開發人員 AD FS 案例"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 753b2b235cb1d73ab47588f8f229410c1f81db40
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-scenarios-for-developers"></a>適用於開發人員 AD FS 案例

>適用於：Windows Server 2016

在 Windows Server 2016 [AD FS 2016] AD FS 可讓您新增的業界標準連接的 OpenID 與 OAuth 2.0 根據驗證與您正在開發，應用程式的授權，並讓使用者直接 AD FS 進行驗證的應用程式。    
  
AD FS 2016 也支援 WS-聯盟 Ws-trust，以及 SAML 通訊協定和設定檔我們已經支援之前的版本。  如果您感興趣的開發人員指南這些通訊協定，查看的文章。  本文會對焦於如何使用及受益較新支援的通訊協定。  
  
## <a name="why-modern-authentication"></a>為何現代化驗證  
時，您可以繼續使用 WS 同盟上使用 AD FS 的登入，Ws-trust，並為您 SAML 通訊協定有之前，較新的通訊協定，您會收到下列優點：  
  
* **簡單和一致性**  
    * 使用相同的 Api 和的登入以便模式：   
        *   多種類型的應用程式 （伺服器、 桌面、 行動裝置版、 瀏覽器）  
        *   多個平台 (android、 iOS，Windows)  
        *   應用程式中的企業網路或裝載的雲端中  
    * 使用您已可使用驗證使用者 Azure AD 的媒體櫃的  
* **彈性**  
    * 除了標準使用者的授權，例如讓更多複雜的案例：      
        * ? 3 腳登入流向一個 web 應用程式或服務的其他網頁應用程式或服務存取資源，使用者的授權。    
        * ? 伺服器-流程中的多層服務存取端的 API  
        * ? JavaScript 以單頁模式應用程式] 選項  
* **Industry 的支援**  
    * OAuth 2.0 和 OpenID 連接享受寬使用量業界，讓您掌握這些模式可協助您讓驗證和以外的 Active Directory 環境授權  
  
## <a name="how-it-works-the-basics"></a>它的運作方式： 的基本資訊  
您可以新增 AD FS 現代化驗證您的應用程式使用的工具，以及您已可使用驗證使用者 Azure AD 的媒體櫃。   
  
AD FS 案例中當然，是 AD FS 並不 Azure AD 作為身分提供者及授權的伺服器。  否則概念會完全相同： 使用者提供的認證，並取得權杖，直接或存取資源介透過。  
  
最基本案例所組成的使用者或 「 資源擁有者 」，存取 web 應用程式的瀏覽器與互動：  
  
![AD FS 適用於開發人員](media/ADFS_DEV_1.png)  
  
Web 應用程式稱為 「 client 」，因為為止資源存取權杖授權伺服器 (AD FS) 的要求。  資源可能裝載本身 web 應用程式，或是那樣 web API 網路或網際網路上的地方。   「 資源擁有者 」 的使用者授權 client web 應用程式提供授權伺服器的憑證會收到該存取預付碼。    
  
## <a name="how-it-works-components"></a>它的運作方式： 元件  
OAuth 2.0 和 OpenID 連接案例中請 AD FS 使用的工具與您使用 Azure AD 時身分提供者的媒體櫃。  這些元件︰  
* Active Directory 驗證媒體櫃 (ADAL): client 的媒體櫃，幫助收集使用者的認證，建立提交權杖要求及擷取的結果權杖。    
* （開放式網路介面.NET） OWIN 介軟體： 時 OWIN 是根據社群專案時，Microsoft 已伺服器的一組側邊的程式庫保護 web 應用程式與 web 連接 OpenID 與 OAuth 2.0 與 Api  
  
這些元件的角色是如下圖所示：  
  
![AD FS 適用於開發人員](media/ADFS_DEV_2.png)  
  
## <a name="modeling-these-scenarios-in-ad-fs-2016"></a>AD FS 2016 中建模這些案例  
  
### <a name="application-groups"></a>應用程式群組  
若要表示 AD FS 原則在這些案例中的，我們已經引入新的概念稱為 「 應用程式群組。  應用程式群組可以包含任何數字和組合應用程式的基本人權以下列類型：  
  
  
  
應用程式群組 / 應用程式類型  |描述  |角色    
---------|---------|---------  
原生應用程式     |  有時稱為公用 client，這被要 client 應用程式執行部電腦或裝置並使用的使用者互動。       | 從授權伺服器 (AD FS) 為使用者存取要求權杖資源。  將 HTTP 要求傳送給受保護的資源，使用權杖 HTTP 標頭。        
伺服器應用程式     |   Web 應用程式的伺服器上執行，並透過瀏覽器使用者通常可以存取。  因為它是維護自己 client '密碼' 或認證的功能，它通常稱為機密 client。      | 從授權伺服器 (AD FS) 為使用者存取要求權杖資源。  將 HTTP 要求傳送給受保護的資源，使用權杖 HTTP 標頭。               
Web API     |  結束資源使用者存取。 這些稱為 「 信賴派對 」 的代表新的想法。| 使用權杖取得用  
  
### <a name="differences-from-ad-fs-2012-r2"></a>AD FS 2012 R2 的不同  
應用程式群組結合信任和授權 AD FS 2012 R2 另行購買，公開信賴派對、 戶端，以及應用程式權限的項目。  
  
下表比較用對應應用程式信任中建立物件 AD FS 2012 R2 與 AD FS 2016 方法：  
  
在 Windows Server 2012 R2 AD FS|在 [PowerShell|AD FS 管理  
---------|---------|---------  
新增原生 client|新增 AdfsClient|NA  
為 client 新增伺服器應用程式|新增 AdfsClient|NA  
新增 Web API / 資源|新增 AdfsRelyingPartyTrust|建立信賴派對信任  
  
AD FS 2016|在 [PowerShell|AD FS 管理  
---------|---------|---------  
新增原生 client|新增 AdfsNativeClientApplication|新增原生應用程式群組  
為 client 新增伺服器應用程式|新增 AdfsServerApplication|新增伺服器應用程式群組  
新增 Web API / 資源|新增 AdfsWebApiApplication|新增 Web API 」 應用程式群組  
  
### <a name="application-permissions-and-consent"></a>應用程式權限和同意  
根據預設，在 [應用程式群組戶端已獲授權存取相同的群組中的資源。  系統管理員不必設定特定應用程式權限。  應用程式群組也可讓系統管理員，若要指定，例如 openid 或 user_impersonation 允許的範圍。  下方的案例描述指定確切的範圍所需的案例。  
  
因為 AD FS 使用的系統管理員同意型號，所以不會提示使用者同意時存取資源。  藉由設定的應用程式群組，系統管理員作用中提供代表所有應用程式使用者同意。  
  
## <a name="supported-scenarios"></a>支援的案例  
下一節告訴您，我們在更多詳細資料中支援的案例。  
  
### <a name="tokens-used"></a>使用發行  
將這些案例中使用權杖的三種類型：  
  
* **id_token:**用來表示的使用者身分 A JWT 預付碼。 Id_token 宣告 'aud' 或對象符合 client ID 的原生或伺服器應用程式。  
* **access_token:**使用 A JWT 權杖 Oauth，OpenID 連接案例和想来使用的資源。  此預付碼 'aud' 或對象宣告必須符合的識別碼或多個 Web API。  
* **refresh_token:**此預付碼提交來取代收集使用者的認證單一登入提供的體驗。  此預付碼是同時發行，並由 AD FS，且目前並非讀取戶端或資源。    
  
### <a name="native-client-to-web-api"></a>原生 client Web api  
本案例可讓使用者的原生 client 應用程式呼叫 AD FS 保護 2016 Web API。  
* 原生 client 應用程式使用 ADAL 傳送授權，並要求 AD FS，並提示您輸入的必要時，使用者的認證預付碼，然後傳送為 HTTP 標頭 Web api 要求的結果權杖  
* [此組件會僅供示範]Web API 讀取宣告從存取權杖傳送 client、 從結果並將其傳送到 client ClaimsPrincipal 物件。  
  
![通訊協定流程的描述](media/ADFS_DEV_3.png)  
  
1.  原生 client 應用程式初始化通話 ADAL 文件庫與流程。  這樣會觸發瀏覽器為基礎 AD FS HTTP 取得授權端點：  
  
**授權要求：**  
取得 https://fs.contoso.com/adfs/oauth2/authorize?  
  
參數|值。  
---------|---------  
response_type|「 程式碼 」  
資源|Web API 應用程式群組中的資源點數 ID （識別碼）  
client_id|client Id 應用程式群組中的原生應用程式  
redirect_uri|重新導向應用程式群組中的應用程式原生的 URI  
  
**授權要求回應：**  
如果使用者有未登入前提示使用者的認證。    
AD FS 回應，以 「 程式碼 」 中的參數 redirect_uri 的查詢元件退貨授權的程式碼。  例如： HTTP 1.1 302 找到位置： **http://redirect_uri:80 日？ 程式碼 =&lt;的程式碼&gt;。**  
  
2.  原生 client 然後將驗證碼，下列參數，以及傳送給 AD FS 權杖端點：  
  
**權杖要求：**  
文章 https://fs.contoso.com/adfs/oauth2/token  
  
參數|值。  
---------|---------  
grant_type|「 authorization_code 」 
程式碼|1 授權的程式碼  
資源|Web API 應用程式群組中的資源點數 ID （識別碼）  
client_id|client Id 應用程式群組中的原生應用程式  
redirect_uri|重新導向應用程式群組中的應用程式原生的 URI  
  
**權杖要求回應：**  
AD FS 使用 access_token、 refresh_token 和本文 id_token HTTP 200 回應。  
  
3.  然後原生的應用程式會 web API，以在 HTTP 要求授權標頭傳送上述回應 access_token 部分。  
  
### <a name="single-sign-on-behavior"></a>單一登入的行為  
1 小時 （預設） access_token 仍然會有效快取，且不會有新的邀請觸發 AD FS 任何流量後續 client 會要求中。  自動將由 ADAL 快取從擷取 access_token。  
  
ADAL 將會自動傳送給 AD FS 權杖端點 （略過授權要求自動） 重新整理權杖根據的要求存取權杖到期之後。  
**重新整理權杖要求：**  
文章 https://fs.contoso.com/adfs/oauth2/token
   

參數|值。|
---------|---------
grant_type|「 refresh_token 」|
資源|Web API 應用程式群組中的資源點數 ID （識別碼）|
client_id|client Id 應用程式群組中的原生應用程式
refresh_token|重新整理權杖發行的初始權杖要求因應日光 AD FS

  
  
**重新整理權杖要求回應：**  
< SSO_period > 在重新整理預付碼時，會在新的憑證存取要求。 使用者不會提示輸入認證。  如需有關 SSO 設定查看[AD FS 單一登入設定](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)  
  
要求重新整理預付碼已過期時，是否會導致錯誤 」 invalid_grant 」 與 「 error_description 「 HTTP 401 」 MSIS9615： 收到 refresh_token 參數重新整理預付碼已過期 」。 在這種情形下，會自動 ADAL 送出新的授權要求看起來很像上述 #1。    
  
### <a name="web-browser-to-web-app"></a>網頁瀏覽器 Web 應用程式   
在本案例中，瀏覽器使用者需要存取資源裝載的 web 應用程式。    
有兩個案例完成這項工作。  
  
#### <a name="oauth-confidential-client"></a>Oauth 機密 client  
本案例是類似上述中已授權的要求，後面權杖換貨的程式碼。  Web 應用程式 （以做為 AD FS 伺服器應用程式） 的初始授權要求透過瀏覽器，並交換權杖的程式碼 （，直接連接 AD FS）  
  
![通訊協定流程的描述](media/ADFS_DEV_4.png)  
  
1.  授權要求傳送給 AD FS 的 [HTTP 取得的瀏覽器，透過 Web 應用程式初始化授權端點  
**要求授權**:  
取得 https://fs.contoso.com/adfs/oauth2/authorize?  
  
參數|值。  
---------|---------  
response_type|「 程式碼 」  
資源|Web API 應用程式群組中的資源點數 ID （識別碼）  
client_id|Client 來電顯示應用程式群組中的原生應用程式  
redirect_uri|重新導向 URI web 應用程式 （伺服器應用程式） 在 [應用程式群組  
  
授權要求回應：  
如果使用者有未登入前提示使用者的認證。  
AD FS 回應，例如 「 程式碼 」 中的參數 redirect_uri 的查詢元件為退貨授權的程式碼： HTTP 日 1.1 302 找到位置： https://webapp.contoso.com/?code=&lt;的程式碼&gt;。  
  
2.  根據上述 302，瀏覽器開始 HTTP 取得 web 應用程式，例如： 取得 http://redirect_uri:80 日？ 程式碼 =&lt;的程式碼&gt;。   
  
3.  Web 應用程式，有收到的驗證碼，此時初始化給 AD FS 權杖端點，傳送下列要求  
**權杖要求：**  
文章 https://fs.contoso.com/adfs/oauth2/token  
  
參數|值。  
---------|---------  
grant_type|「 authorization_code 」  
程式碼|從 2 上述授權的程式碼  
資源|Web API 應用程式群組中的資源點數 ID （識別碼）  
client_id|Client 來電顯示的應用程式群組中的 web 應用程式 （伺服器應用程式）  
redirect_uri|重新導向 URI web 應用程式 （伺服器應用程式） 在 [應用程式群組  
client_secret|Web 應用程式 （伺服器應用程式） 中的應用程式群組的密碼。 **注意： Client 的認證不需要將 client_secret。  AD FS 支援，以及使用憑證] 或 [Windows 整合式驗證的能力。**  
  
**權杖要求回應：**  
AD FS 使用 access_token、 refresh_token 和本文 id_token HTTP 200 回應。  
宣告  
4.  網站或應用程式可能會消耗 access_token 回應的一部分上述 （範例中的 [網路] app 本身主控資源），然後將它傳送為 HTTP 要求授權首 web API。  
  
#### <a name="single-sign-on-behavior"></a>單一登入的行為  
同時存取權杖仍有效的 1 小時 （預設） client 的快取中，您可能會認為的第二個要求能與上述的原生 client 案例相同的新的邀請將不會觸發 AD FS 任何流量為存取預付碼將會自動讀取的快取 ADAL 來。  不過，則可能 web 應用程式可傳送不同授權和權杖要求，透過不同的 URL 先前的連結，如範例所示。  
  
這是 AD FS 瀏覽器 SSO cookie 可讓 AD FS 不會提示使用者提供的認證發出新授權的程式碼。 Web 應用程式，然後 AD FS 對換貨新的程式碼授權取得新的憑證存取。  使用者不會提示輸入認證。  
  
或者，如果 web 應用程式智慧足以知道是否已驗證使用者時，可以略過授權要求和任一：  
* 快取的存取預付碼未過期，如果是擷取和使用，或   
* 可以要求權杖根據的要求傳送給 AD FS 權杖端點，如下所述  
  
**重新整理權杖要求：**  
文章 https://fs.contoso.com/adfs/oauth2/token
   
參數|值。  
---------|---------  
grant_type|「 refresh_token 」  
資源|Web API 應用程式群組中的資源點數 ID （識別碼）  
client_id|Client 來電顯示的應用程式群組中的 web 應用程式 （伺服器應用程式）  
refresh_token|重新整理發行的初始權杖要求因應日光 AD FS 預付碼  
client_secret|Web 應用程式 （伺服器應用程式） 中的應用程式群組的密碼  
  
**重新整理權杖要求回應：**  
< SSO_period > 在重新整理預付碼時，會在新的憑證存取要求。 使用者不會提示輸入認證。 如需有關 SSO 設定查看[AD FS 單一登入設定](../../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)   
  
要求重新整理預付碼已過期時，是否會導致錯誤 」 invalid_grant 」 與 「 error_description 「 HTTP 401 」 MSIS9615： 收到 refresh_token 參數重新整理預付碼已過期 」。 在這種情形下，會自動 ADAL 送出新的授權要求看起來很像上述 #1。    
  
#### <a name="openid-connect-hybrid-flow"></a>OpenID 連接： 混合流程  
本案例中有類似上述的授權要求起始網頁瀏覽器重新導向，以及權杖 exchange AD FS 來自 web 應用程式的程式碼 app。  本案例中的不同的是，AD FS 問題 id_token 初始授權要求回應的一部分。  
  
![通訊協定流程的描述](media/ADFS_DEV_5.png)  
  
1.  授權要求傳送給 AD FS 的 [HTTP 取得的瀏覽器，透過 Web 應用程式初始化授權端點  
  
**授權要求：**  
取得 https://fs.contoso.com/adfs/oauth2/authorize?  
  
參數|值。  
---------|---------  
response_type|「 程式碼 + id_token]  
response_mode|「 form_post 」  
資源|Web API 應用程式群組中的資源點數 ID （識別碼）  
client_id|Client 來電顯示的應用程式群組中的 web 應用程式 （伺服器應用程式）  
redirect_uri|重新導向 URI web 應用程式 （伺服器應用程式） 中的應用程式群組  
  
**授權要求回應：**  
如果使用者有未登入前提示使用者的認證。  
AD FS 看 HTTP 200 和表單包含下列為隱藏的項目：  
* 程式碼： 授權的程式碼  
* id_token: JWT 權杖包含描述使用者驗證宣告  
2.  表單自動將張貼到 redirect_uri web 應用程式，傳送驗證碼與 id_token web 應用程式。  
  
3.  Web 應用程式，有收到的驗證碼，此時初始化給 AD FS 權杖端點，傳送下列要求  
  
**權杖要求：**  
文章 https://fs.contoso.com/adfs/oauth2/token
  
  
  
參數|值。  
---------|---------  
grant_type|「 authorization_code 」  
程式碼|上述授權的程式碼  
資源|Web API 應用程式群組中的資源點數 ID （識別碼）  
client_id|Client 來電顯示的應用程式群組中的 web 應用程式 （伺服器應用程式）  
redirect_uri|重新導向 URI web 應用程式 （伺服器應用程式） 在 [應用程式群組  
client_secret|Web 應用程式 （伺服器應用程式） 中的應用程式群組的密碼  
  
**權杖要求回應：**  
AD FS 使用 access_token、 refresh_token 和本文 id_token HTTP 200 回應。  
  
4.  網站或應用程式可能會消耗 access_token 回應的一部分上述 （範例中的 [網路] app 本身主控資源），然後將它傳送為 HTTP 要求授權首 web API。  
  
#### <a name="single-sign-on-behavior"></a>單一登入的行為  
單一登入行為是與 Oauth 2.0 機密 client 流程上述相同。  
  
### <a name="on-behalf-of"></a>代表  
在本案例中，web 應用程式會使用原始存取權杖使用者要求和其他 web 應用程式將會再存取與使用者的 Web api 取得其他存取預付碼。  這稱為 」 的代表的 「 流程。  
  
![通訊協定流程的描述](media/ADFS_DEV_6.png)  
  
步驟 1 到 2 一樣步驟 3、 4 中的上一個流程。  
在執行 「 步驟 3 金鑰需求是 client_id 參數，client ID Web 應用程式 2，必須符合資源點數 ID 的 Web API a。亦即的對象交換取得新的憑證存取權杖必須符合實體要求的新權杖 client 的 ID。  

## <a name="related-content"></a>相關的 content  
查看[AD FS 開發](../AD-FS-Development.md)的完整清單解說文章中，這將提供逐步指示上使用的相關的流程。 
