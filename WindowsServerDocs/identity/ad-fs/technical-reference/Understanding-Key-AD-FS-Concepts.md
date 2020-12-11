---
description: 深入瞭解：瞭解重要的 AD FS 概念
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: 瞭解重要 Active Directory 同盟服務概念
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0e31dfea3521fbbaac11f6fe5c1ae3666637118a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050456"
---
# <a name="understanding-key-ad-fs-concepts"></a>Understanding Key AD FS Concepts
建議您瞭解 Active Directory 同盟服務的重要概念，並熟悉其功能集。

> [!TIP]
> 您可以在 [瞭解重要 AD FS 概念]()中找到其他 AD FS 資源連結。

## <a name="ad-fs-terminology-used-in-this-guide"></a>此指南中使用的 AD FS 詞彙

|AD FS 詞彙|定義|
|--------------|--------------|
|帳戶夥伴組織|由 Federation Service 中之宣告提供者信任代表的同盟夥伴組織。 帳戶夥伴組織包含的使用者將存取 \- 資源夥伴中的 Web 應用程式。|
|帳戶同盟伺服器|帳戶夥伴組織中的同盟伺服器。 帳戶同盟伺服器會根據使用者驗證簽發安全性權杖。 伺服器會驗證使用者、將相關的屬性和群組成員資格資訊解壓縮至屬性存放區，並將此資訊封裝到宣告中，然後產生並簽署 \( 包含要傳回給使用者之宣告的安全性權杖， \) 以在自己的組織中使用，或傳送給夥伴組織。|
|AD FS 設定資料庫|用來儲存代表單一 AD FS 執行個體或 Federation Service 之所有設定資料的資料庫。 此設定資料可以儲存在 SQL Server 資料庫中，或使用 Windows Server 2016、Windows Server 2012 和 2012 R2 隨附的 Windows 內部資料庫功能，以及 Windows Server 2008 和 2008 R2。 </br></br>您可以使用 Fsconfig.exe 命令列工具建立 SQL Server 的 AD FS 設定資料庫 \- ，以及使用 AD FS 同盟伺服器設定向導 Windows 內部資料庫。|
|宣告提供者|提供宣告給其使用者的組織。 請參閱＜帳戶夥伴組織＞。|
|宣告提供者信任|在 AD FS 管理嵌入式管理單元 \- 中，宣告提供者信任是通常在資源夥伴組織中建立的信任物件，代表信任關係中的組織，其帳戶將會存取資源夥伴組織中的資源。 宣告提供者信任物件由各種識別碼、名稱與規則組成，可讓本機 Federation Service 識別此夥伴。|
|本機宣告提供者信任|信任物件，代表 \- AD FS 伺服器陣列中 AD LDS 或協力廠商 LDAP 型 \- 目錄。 本機宣告提供者信任物件是由各種不同的識別碼、名稱和規則所組成，可識別本機同盟服務的這個 LDAP 型 \- 目錄。|
|同盟中繼資料|一種資料格式，此資料格式用於在宣告提供者與信賴憑證者之間溝通設定資訊，以進行適當的宣告提供者信任與信賴憑證者信任設定。 資料格式是在安全性聲明標記語言 \( SAML 2.0 中定義 \) ，而且在 WS 同盟中是延伸的 \- 。|
|同盟伺服器|已使用 AD FS Federation Server Configuration Wizard 設定的 Windows Server，可在同盟伺服器角色中採取動作。 同盟伺服器會簽發權杖並做為 Federation Service 的一部分。|
|同盟伺服器 Proxy|使用 AD FS 同盟伺服器 Proxy 設定向導設定的 Windows 伺服器，可作為網際網路用戶端與位於公司網路防火牆後方的同盟服務之間的中繼 Proxy 服務。|
|主要同盟伺服器|已使用 AD FS Federation Server Configuration Wizard 設定于同盟伺服器角色中的 Windows Server，且具有 AD FS 設定資料庫的讀取 \/ 寫入複本。 </br></br> 當您使用 AD FS 同盟伺服器設定向導並選取建立新同盟服務的選項，並讓該電腦成為伺服器陣列中的第一部同盟伺服器時，就會建立主要同盟伺服器。 此伺服器陣列中的所有其他同盟伺服器，都必須將主要同盟伺服器上所做的變更複寫到 \- 儲存在本機的 AD FS 設定資料庫的唯讀複本。 當 AD FS 設定資料庫儲存在 SQL database 中時，「主要同盟伺服器」一詞並不適用，因為所有同盟伺服器都可以同樣地讀取和寫入儲存在 SQL Server 上的設定資料庫。|
|信賴憑證者|接收並處理宣告的組織。 請參閱＜資源夥伴組織＞。|
|信賴憑證者信任|在 AD FS 管理] 嵌入式管理單元 \- 中，信賴憑證者信任是通常在中建立的信任物件：<p>-帳戶夥伴組織代表信任關係中的組織，其帳戶將會存取資源夥伴組織中的資源。<br />-資源夥伴組織，以代表同盟服務與單一 web 應用程式之間的信任 \- 。<p>信賴憑證者信任物件是由各種識別碼、名稱和規則所組成，可將此夥伴或 web \- 應用程式識別至本機同盟服務。|
|資源同盟伺服器|資源夥伴組織中的同盟伺服器。 資源同盟伺服器通常會根據帳戶同盟伺服器簽發的安全性權杖來簽發安全性權杖給使用者。 伺服器會接收安全性權杖、驗證簽章、將宣告規則邏輯套用至未封裝的宣告，以產生所需的傳出宣告、 \( 根據傳入安全性權杖中的資訊，以傳出宣告產生新的安全性權杖， \) 並將新的權杖登入以返回使用者，最後再登入至 Web 應用程式。|
|資源夥伴組織|由  Federation Service 中之信賴憑證者信任所代表的同盟夥伴。 資源夥伴會發出以宣告為 \- 基礎的安全性權杖，其中包含 \- 帳戶夥伴中的使用者可以存取的已發佈 Web 應用程式。|

## <a name="overview-of-ad-fs"></a>AD FS 概觀
AD FS 是一種身分識別存取解決方案，可讓 \( 您的網路內部或外部的用戶端電腦 \) 順暢地存取受保護的網際網路對向 \- 應用程式或服務，即使使用者帳戶和應用程式位於完全不同的網路或組織中也一樣。

當應用程式或服務位於某個網路，而使用者帳戶位於另一個網路，一般而言，當使用者嘗試存取應用程式或服務時，系統會提示使用者提供第二個認證。 這些第二個認證代表應用程式或服務所在網路的使用者身分識別。 裝載應用程式或服務的網頁伺服器通常會要求使用者提供第二個認證，以便進行最適當的授權決策。

藉由 AD FS，組織可以透過提供信任關係 \( 同盟信任 \) ，讓這些組織可以用來將使用者的數位身分識別和存取權限投射到信任的夥伴，以略過次要認證的要求。 在此同盟環境中，每個組織會繼續管理其自己的身分識別，但每個組織也都可以安全地投射並接受來自其他組織的身分識別。

-   [屬性存放區的角色](The-Role-of-Attribute-Stores.md)

-   [AD FS 設定資料庫的角色](The-Role-of-the-AD-FS-Configuration-Database.md)

-   [宣告的角色](The-Role-of-Claims.md)

-   [宣告規則的角色](The-Role-of-Claim-Rules.md)

-   [宣告引擎的角色](The-Role-of-the-Claims-Engine.md)

-   [宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)

-   [宣告規則語言的角色](The-Role-of-the-Claim-Rule-Language.md)

-   [判斷要使用的宣告規則範本類型](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)

-   [AD FS 中的 URI 使用方式](How-URIs-Are-Used-in-AD-FS.md)
