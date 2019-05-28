---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: 了解金鑰的 Active Directory Federation Services 概念
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac666539170bb7aabf0b7f58a7ef003ebe87c2a8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188389"
---
# <a name="understanding-key-ad-fs-concepts"></a>了解重要的 AD FS 概念
建議您深入了解的重要概念，如 Active Directory Federation Services，並熟悉其功能集。  
  
> [!TIP]  
> 您可以在 Microsoft TechNet Wiki 上的 [AD FS 內容地圖](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) 頁面找到其他 AD FS 資源連結。 此頁面是由 AD FS 社群的成員管理，而且 AD FS 產品小組會不定期監控此頁面。  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>此指南中使用的 AD FS 詞彙  
  
|AD FS 詞彙|定義|  
|--------------|--------------|  
|帳戶夥伴組織|由 Federation Service 中之宣告提供者信任代表的同盟夥伴組織。 帳戶夥伴組織包含會存取 Web 的使用者\-資源夥伴中的應用程式。|  
|帳戶同盟伺服器|帳戶夥伴組織中的同盟伺服器。 帳戶同盟伺服器會根據使用者驗證簽發安全性權杖。 伺服器會驗證使用者，擷取相關的屬性和屬性存放區的群組成員資格資訊、 此資訊封裝到宣告、 產生和簽署安全性權杖\(其中包含宣告\)若要傳回給使用者 — 要用於自己的組織，或傳送給夥伴組織。|  
|AD FS 設定資料庫|用來儲存代表單一 AD FS 執行個體或 Federation Service 之所有設定資料的資料庫。 這項組態資料可以儲存在 SQL Server 資料庫，或使用 Windows 內部資料庫 」 功能隨附於 Windows Server 2016、 Windows Server 2012 和 2012 R2 和 Windows Server 2008 和 2008 R2。 </br></br>您可以使用 Fsconfig.exe 命令的 SQL server 中建立的 AD FS 設定資料庫\-列工具並使用 AD FS 同盟伺服器設定精靈的 Windows 內部資料庫。|  
|宣告提供者|提供宣告給其使用者的組織。 請參閱＜帳戶夥伴組織＞。|  
|宣告提供者信任|在 AD FS 管理嵌入式管理單元\-中，宣告提供者信任的信任物件通常建立來代表該組織的信任關係中的資源夥伴組織中的帳戶將會存取資源的資源夥伴組織。 宣告提供者信任物件由各種識別碼、名稱與規則組成，可讓本機 Federation Service 識別此夥伴。|  
|本機宣告提供者信任|信任物件，代表 AD LDS 或第三個\-方 LDAP\-基礎 AD FS 伺服器陣列中的目錄。 本機宣告提供者信任物件包含的各種不同的識別項、 名稱和規則來識別此 LDAP\-基礎讓本機 Federation Service 的目錄。|  
|同盟中繼資料|一種資料格式，此資料格式用於在宣告提供者與信賴憑證者之間溝通設定資訊，以進行適當的宣告提供者信任與信賴憑證者信任設定。 安全性聲明標記語言中定義的資料格式\(SAML\) 2.0 中，而且它將會擴充 WS-I 中\-同盟。|  
|同盟伺服器|已使用 AD FS 同盟伺服器設定精靈，以扮演同盟伺服器角色的 Windows 伺服器。 同盟伺服器會簽發權杖並做為 Federation Service 的一部分。|  
|同盟伺服器 Proxy|已使用 AD FS 同盟伺服器 Proxy 設定精靈，以做為 Windows Server 的中繼 proxy 服務之間的網際網路用戶端與位於公司網路防火牆後方之 Federation Service。|  
|主要同盟伺服器|Windows Server 已設定同盟伺服器角色，使用 AD FS 同盟伺服器設定精靈中，有讀取\/寫入 AD FS 設定資料庫的複本。 </br></br> 當您使用 AD FS 同盟伺服器設定精靈，並選取要建立新的 Federation Service，並將該電腦的設定，第一部同盟伺服器陣列中的選項，會建立在主要同盟伺服器。 此伺服器陣列中的所有其他同盟伺服器必須複寫到讀取主要同盟伺服器上所做的變更\-只會儲存在本機的 AD FS 設定資料庫複本。 當 AD FS 設定資料庫是儲存在 SQL 資料庫中時，「主要同盟伺服器」一詞不適用，因為在此情況下，所有同盟伺服器都能讀取及寫入儲存在 SQL Server 上的設定資料庫。|  
|信賴憑證者|接收並處理宣告的組織。 請參閱＜資源夥伴組織＞。|  
|信賴憑證者信任|在 AD FS 管理嵌入式管理單元\-中，信賴憑證者信任是通常在中建立的信任物件：<br /><br />來代表該組織的帳戶存取資源，資源夥伴組織中的信任關係中的帳戶夥伴組織。<br />資源夥伴組織來代表 Federation Service 之間的單一 web 信任\-基礎的應用程式。<br /><br />信賴憑證者信任物件包含各種不同的識別項、 名稱和規則來識別此夥伴或 web\-讓本機 Federation Service 的應用程式。|  
|資源同盟伺服器|資源夥伴組織中的同盟伺服器。 資源同盟伺服器通常會根據帳戶同盟伺服器簽發的安全性權杖來簽發安全性權杖給使用者。 伺服器接收安全性權杖、 驗證簽章、 適用於宣告規則邏輯的未封裝的宣告，以產生所需的連出宣告、 產生新的安全性權杖\(與連出宣告\)根據資訊在連入的安全性權杖中，並簽署新的權杖傳回給使用者且最終在 Web 應用程式。|  
|資源夥伴組織|由  Federation Service 中之信賴憑證者信任所代表的同盟夥伴。 資源夥伴會簽發宣告\-基礎的安全性權杖，其中包含已發佈的 Web\-帳戶夥伴中的使用者可以存取的應用程式。|  
  
## <a name="overview-of-ad-fs"></a>AD FS 概觀  
AD FS 是提供用戶端電腦的身分識別存取解決方案\(內部或外部網路\)無縫式 SSO 存取受保護的網際網路\-面向的應用程式或服務，即使使用者帳戶和應用程式位於相同網路或組織。  
  
當應用程式或服務位於某個網路，而使用者帳戶位於另一個網路，一般而言，當使用者嘗試存取應用程式或服務時，系統會提示使用者提供第二個認證。 這些第二個認證代表應用程式或服務所在網路的使用者身分識別。 裝載應用程式或服務的網頁伺服器通常會要求使用者提供第二個認證，以便進行最適當的授權決策。  
  
使用 AD FS 時，組織可以透過提供信任關係免除要求第二個認證\(同盟信任\)這些組織可以使用使用者的數位身分識別與存取權限投射到信任合作夥伴。 在此同盟環境中，每個組織會繼續管理其自己的身分識別，但每個組織也都可以安全地投射並接受來自其他組織的身分識別。  
  
-   [角色的屬性存放區](The-Role-of-Attribute-Stores.md)  
  
-   [AD FS 設定資料庫的角色](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [宣告的角色](The-Role-of-Claims.md)  
  
-   [宣告規則的角色](The-Role-of-Claim-Rules.md)  
  
-   [宣告引擎的角色](The-Role-of-the-Claims-Engine.md)  
  
-   [宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)  
  
-   [宣告規則語言的角色](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [判斷要使用的宣告規則範本類型](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [AD FS 中的 URI 使用方式](How-URIs-Are-Used-in-AD-FS.md)  
  

