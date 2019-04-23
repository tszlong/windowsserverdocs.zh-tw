---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: 宣告的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: df35348cb51a9021f4aaa2fc6516cb119bdb8b8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849429"
---
>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

# <a name="the-role-of-claims"></a>宣告的角色
宣告中\-型的識別模型宣告同盟程序中扮演著關鍵角色，它們是由其中的重要元件的所有 Web 結果\-判斷基礎的驗證和授權要求。 此模型可讓組織以標準化方式跨安全性與企業界限安全地投射數位身分識別與權限或「宣告」。  
  
## <a name="what-are-claims"></a>什麼是宣告？  
最簡單的形式，在宣告是只要*陳述式* \(，例如名稱、 識別、 群組\)，提出有關使用者的主要用來授權存取宣告\-架構的應用程式位於網際網路上的任何位置。 每個聲明都對應到宣告中儲存的「值」。  
  
### <a name="how-claims-are-sourced"></a>如何為宣告進行溯源  
Active Directory Federation Services 中的 Federation Service \(AD FS\)定義同盟夥伴之間交換的宣告。 不過，在可以這樣做之前，它必須使用抓取的值或計算的值來填入宣告或為宣告進行溯源。 每個宣告值都代表使用者、群組或實體的值，而且能讓您以下列兩種方式之一進行溯源：  
  
1.  當構成宣告的值是從屬性存放區中擷取時 (例如當「銷售部門」的屬性值是從 Active Directory 使用者帳戶的屬性擷取)。 如需詳細資訊，請參閱[屬性存放區的角色](The-Role-of-Attribute-Stores.md)。  
  
2.  當連入宣告的值根據規則中表示的邏輯轉換到另一個值時。 例如，當具有 Domain Admins 值的連入宣告在以連出宣告形式傳送之前被轉換到新值 Administrators。 如需詳細資訊，請參閱[宣告規則的角色](The-Role-of-Claim-Rules.md)。  
  
宣告可以包含值，例如電子\-地址，使用者主體名稱\(UPN\)，群組成員資格以及其他帳戶屬性。  
  
### <a name="how-claims-flow"></a>宣告的流程  
其他實體依賴宣告 for Web 中執行授權工作的值為\-其裝載的應用程式。 這些合作對象指*信賴憑證者的合作對象*AD FS 管理嵌入式管理單元中\-中。 Federation Service 負責仲介許多不同實體之間的信任。 它旨在處理方向，從最初的宣告，也稱為組織宣告信任宣告交換*宣告提供者*AD FS 管理嵌入式管理單元中\-在中，信賴憑證者的合作對象。 接著，信賴憑證者會使用這些宣告來做出授權決策。  
  
使用此程序的宣告傳遞流程稱為「宣告管線」。 宣告在宣告管線中的傳遞流程有三個步驟：  
  
1.  從宣告提供者接收的宣告會由宣告提供者信任上的接受轉換規則處理。 這些規則決定會接受來自宣告提供者的哪些宣告。  
  
2.  接受轉換規則的輸出會用來做為發佈授權規則的輸入。 這些規則決定是否允許使用者存取信賴憑證者。  
  
3.  接受轉換規則的輸出會用來做為發佈轉換規則的輸入。 這些規則決定將傳送到信賴憑證者的宣告。  
  
如需詳細資訊，請參閱[宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>宣告的發佈方式  
當您撰寫宣告規則時，宣告規則的連入宣告來源是根據您正在撰寫宣告提供者信任規則或信賴憑證者信任規則而定。 當您撰寫宣告提供者信任的宣告規則時，連入宣告是從信任的宣告提供者傳送到 Federation Service 的宣告。 當您撰寫信賴憑證者信任的規則時，連入宣告是適用之宣告提供者信任之宣告規則所輸出的宣告。 如需有關連入宣告與連出宣告的詳細資訊，請參閱 [宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)與[宣告引擎的角色](The-Role-of-the-Claims-Engine.md)。  
  
## <a name="what-are-claim-types"></a>什麼是宣告類型？  
宣告類型提供宣告值的上下文資訊。 它通常會以統一資源識別項\(URI\)。 AD FS 可以支援任何宣告類型，而且會依預設設定下表中的宣告類型。  
  
|名稱|描述|URI|  
|--------|---------------|-------|  
|E\-郵件地址|E\-郵件地址的使用者|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress|  
|名字|使用者的名字|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenname|  
|名稱|使用者的唯一名稱|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/name|  
|UPN|使用者主體名稱\(UPN\)的使用者|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/upn|  
|一般名稱|使用者的一般名稱。|http:\/\/schemas.xmlsoap.org\/claims\/CommonName|  
|AD FS 1.x 電子\-郵件地址|E\-與 AD FS 1.1 或 ADFS 1.0 搭配使用時，郵件地址的使用者|http:\/\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|群組|使用者所屬的群組|http:\/\/schemas.xmlsoap.org\/宣告\/群組|  
|AD FS 1.x UPN|與 AD FS 1.1 或 ADFS 1.0 搭配使用時的使用者 UPN|http:\/\/schemas.xmlsoap.org\/claims\/UPN|  
|[角色]|使用者擁有的角色|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/role|  
|姓氏|使用者的姓氏|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|使用者的私人識別碼|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|名稱識別碼|使用者的 SAML 名稱識別碼|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|驗證方法|用來驗證使用者的方法|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/authenticationmethod|  
|僅拒絕群組 SID|拒絕\-只有群組之使用者的 SID|http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|僅拒絕主要 SID|拒絕\-只有使用者的主要 SID|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|僅拒絕主要群組 SID|拒絕\-只有主要群組 SID 的使用者|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|群組 SID|使用者的群組 SID|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|主要群組 SID|使用者的主要群組 SID|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|主要 SID|使用者的主要 SID|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Windows 帳戶名稱|以使用者的網域帳戶名稱\<網域\>\\\<使用者\>|http:\/\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>什麼是宣告描述？  
宣告描述代表 AD FS 支援，可能會刊登在同盟中繼資料的宣告類型的清單。 上表中所述的宣告類型會設定為 AD FS 管理嵌入式管理單元中的宣告描述\-中。  
  
將發行到同盟中繼資料的宣告描述集合是儲存在 AD FS 設定資料庫中。 這些宣告描述會由 Federation Service 的各元件使用。  
  
每個宣告描述都包含宣告類型 URI、名稱、發行狀態與描述。 您可以使用來管理宣告描述集合**宣告描述**AD FS 管理嵌入式管理單元中的節點\-中。 您可以修改宣告描述使用嵌入式管理單元的發行狀態\-中。 可用的設定如下：  
  
-   **在 同盟中繼資料中發行此宣告為可接受此 Federation Service 的宣告型別**\(發行為已接受\)— 表示此同盟將會接受來自其他宣告提供者的宣告類型服務。  
  
-   **在 同盟中繼資料中發行此宣告為此同盟服務可傳送的宣告型別**\(發行為已傳送\)— 表示由此 Federation service 所提供的宣告類型。 這些是 Federation Service 發行至其他宣告提供者且其他宣告提供者願意傳送的宣告類型。 此宣告提供者傳送的實際宣告類型通常是此清單的子集。  
  
如需如何設定宣告類型的發行狀態的詳細資訊，請參閱[新增宣告描述](https://technet.microsoft.com/library/dd807051.aspx)AD FS 部署指南中。  
  
### <a name="when-generating-federation-metadata"></a>同盟中繼資料產生時機  
同盟中繼資料包含標示為待發行的所有宣告描述。  
  
### <a name="when-claims-rules-are-processed"></a>宣告規則處理時機  
當您保留宣告描述的相關設定資訊時，設定宣告相關規則將會比較簡單。 如需有關可在宣告提供者組織中使用之宣告規則的詳細資訊，請參閱[宣告規則的角色](The-Role-of-Claim-Rules.md)。  
  

