---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: "了解金鑰 Active Directory 同盟服務概念"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>了解金鑰 AD FS 概念
建議您了解重要概念 Active Directory 同盟服務熟悉其功能設定。  
  
> [!TIP]  
> 您可以找到其他 AD FS 資源連結，以[AD FS 內容地圖](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx)頁面上的 Microsoft TechNet Wiki。 此頁面由 AD FS 社群的成員，並會定期監視 AD FS Product 小組。  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>本指南使用 AD FS 詞彙  
  
|AD FS 詞彙|解析度|  
|--------------|--------------|  
|Account 合作夥伴公司|聯盟合作夥伴組織所代表宣告同盟服務提供者信任。 Account 合作夥伴公司包含會存取 Web\ 為基礎的資源協力廠商應用程式的使用者。|  
|Account 聯盟伺服器|聯盟 account 合作夥伴組織伺服器。 Account 聯盟伺服器問題的安全性權杖根據驗證使用者的使用者。 伺服器驗證使用者、擷取相關屬性與屬性存放區的群組成員資格資訊、套件宣告，此資訊會產生及簽署的安全性權杖 \（包含 claims\）回到使用者，在自己的組織中使用或傳送到協力廠商的組織。|  
|AD FS 設定資料庫|資料庫用來儲存的所有設定資料，表示單一 AD FS 執行個體或同盟服務。 此組態資料 SQL Server 資料庫中可以儲存或使用 Windows 內部資料庫功能隨附在 Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2。 </br></br>您可以建立 AD FS 設定資料庫 SQL Server 使用 Fsconfig.exe command\ 列工具和 Windows 內部資料庫中使用 AD FS 聯盟伺服器設定精靈。|  
|宣告提供者|提供給使用者宣告組織。 查看 account 合作夥伴組織。|  
|宣告提供者信任|Snap\ 中 AD FS 管理，宣告提供者信任的資源合作夥伴代表組織信任關係資源合作夥伴組織中的資源將會存取其帳號中通常會建立信任物件。 宣告提供者信任物件組成各種識別碼、名稱，以及找出這合作夥伴到本機同盟服務的規則。|  
|本機宣告提供者信任|信任物件代表 AD LDS 或 third\ 廠商 LDAP\ 型目錄 AD FS 發電廠中。 本機宣告提供者信任物件包含許多不同的識別碼、名稱，以及找出到本機同盟服務此 LDAP\ 型 directory 規則。|  
|聯盟中繼資料|資料格式聯繫設定宣告提供者和之前的宣告提供者信任和信賴的派對信任的正確設定信賴之間的資訊。 資料格式安全性判斷提示標記語言 \(SAML\) 2.0，以定義和延伸 WS\-同盟。|  
|聯盟伺服器|Windows Server AD FS 聯盟伺服器設定精靈使用聯盟伺服器角色做已設定。 聯盟伺服器問題權杖，做為同盟服務的一部份。|  
|聯盟伺服器 proxy|Windows Server AD FS 聯盟伺服器 Proxy 設定精靈使用做為已設定中間 proxy 服務網際網路 client 之間位於公司網路上有防火牆同盟服務。|  
|主要聯盟伺服器|Windows Server 已聯盟伺服器角色使用 AD FS 聯盟伺服器設定精靈中，並具有 AD FS 設定資料庫 read\/寫入複本。 </br></br> 當您使用 AD FS 聯盟伺服器設定精靈，並選取 [建立新的同盟服務，以及陣列中將該電腦的第一個聯盟伺服器建立主要聯盟伺服器。 所有其他聯盟伺服器此必須複製僅限 read\ 複本儲存在本機 AD FS 設定資料庫主要聯盟伺服器上所做的變更。 字詞「主要聯盟伺服器」不適用於所有聯盟伺服器同樣讀取並儲存在 SQL Server 設定資料庫寫入 AD FS 設定資料庫儲存 SQL 資料庫中。|  
|仰賴派對|組織接收並處理主張。 查看資源合作夥伴組織。|  
|可以廠商信任|AD FS 管理 snap\ 中，可以廠商信任信任物件通常被建立中：<br /><br />-Account 合作夥伴代表組織中信任關係的帳號存取資源合作夥伴組織中的資源。<br />-資源代表同盟服務與 web\ 為基礎的單一應用程式之間的信任的合作夥伴組織。<br /><br />信賴的派對信任物件組成各種識別碼、名稱，以及找出這協力廠商或到本機同盟服務 web\ 應用程式規則。|  
|資源聯盟伺服器|資源合作夥伴組織中聯盟伺服器。 資源聯盟伺服器通常問題的安全性權杖給使用者根據發出 account 聯盟伺服器的安全性權杖。 伺服器收到的安全性權杖，確認簽章，適用於製作您想要傳出宣告 unpackaged 宣告理賠要求規則邏輯操作，產生新的安全性權杖 \（傳出 claims\) 為基礎中收到的安全性權杖、資訊和簽署新回到使用者權杖，最後的 Web 應用程式。|  
|資源合作夥伴公司|聯盟合作夥伴由信賴廠商信任同盟服務中。 資源合作夥伴問題 claims\ 為基礎的安全性權杖包含發行的 Web\ 為基礎的應用程式中 account 合作夥伴使用者都可以存取。|  
  
## <a name="overview-of-ad-fs"></a>AD FS 的概觀  
AD FS 是提供 client 電腦的身分存取方案 \（內部或外部您 network\）順暢 SSO 存取受保護的 Internet\ 攝影機的應用程式或服務，即使帳號，應用程式位於完全不同的網路或組織中使用。  
  
當應用程式或服務是一個網路而在其他網路帳號時，通常會提示使用者次要認證對方嘗試存取應用程式或服務時。 這些次要認證代表使用者的身分領域的應用程式或服務的所在位置中。 它們通常需要 Web 伺服器，讓它可以最適合授權裝載的應用程式或服務。  
  
AD FS 使用組織可以略過次要認證要求提供信任關係 \(federation trusts\) 這些組織可供投影使用者的數位身分及存取權限受信任合作夥伴。 在這個聯盟環境中，每個組織管理自己的身分，會繼續，但每個組織也確實專案和接受其他組織的身分。  
  
-   [此屬性存放區的角色](The-Role-of-Attribute-Stores.md)  
  
-   [AD FS 設定資料庫的角色](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [宣告的角色](The-Role-of-Claims.md)  
  
-   [宣告規則的角色](The-Role-of-Claim-Rules.md)  
  
-   [宣告引擎的角色](The-Role-of-the-Claims-Engine.md)  
  
-   [宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)  
  
-   [宣告規則語言的角色](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [判斷理賠要求規則範本使用類型](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [AD FS 中使用 Uri 的方式](How-URIs-Are-Used-in-AD-FS.md)  
  

