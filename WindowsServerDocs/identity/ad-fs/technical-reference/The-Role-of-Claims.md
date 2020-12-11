---
description: 深入瞭解：宣告的角色
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: 宣告的角色
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 853689a601f8729fb79091cb1bd7c7c45652f480
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045286"
---
# <a name="the-role-of-claims"></a>宣告的角色

在宣告式身分 \- 識別模型中，宣告會在同盟程式中扮演 pivotal 角色，這是決定所有 Web 型 \- 驗證和授權要求結果的主要元件。 此模型可讓組織以標準化方式跨安全性與企業界限安全地投射數位身分識別與權限或「宣告」。

## <a name="what-are-claims"></a>什麼是宣告？

就最簡單的形式而言，宣告就是簡單的 *語句* \( ，例如，名稱、身分識別、群組 \) ，以及使用者的存取權，主要是用來授權存取 \- 位於網際網路上任何位置的宣告式應用程式。 每個聲明都對應到宣告中儲存的「值」。

### <a name="how-claims-are-sourced"></a>如何為宣告進行溯源

Active Directory 同盟服務 AD FS 中的同盟服務定義在同盟 \( \) 夥伴之間交換的宣告。 不過，在可以這樣做之前，它必須使用抓取的值或計算的值來填入宣告或為宣告進行溯源。 每個宣告值都代表使用者、群組或實體的值，而且能讓您以下列兩種方式之一進行溯源：

1.  當構成宣告的值是從屬性存放區中擷取時 (例如當「銷售部門」的屬性值是從 Active Directory 使用者帳戶的屬性擷取)。 如需詳細資訊，請參閱[屬性存放區的角色](The-Role-of-Attribute-Stores.md)。

2.  當連入宣告的值根據規則中表示的邏輯轉換到另一個值時。 例如，當具有 Domain Admins 值的連入宣告在以連出宣告形式傳送之前被轉換到新值 Administrators。 如需詳細資訊，請參閱[宣告規則的角色](The-Role-of-Claim-Rules.md)。

宣告可以包含像是電子 \- 郵寄地址、使用者主體名稱 \( UPN \) 、群組成員資格和其他帳戶屬性的值。

### <a name="how-claims-flow"></a>宣告的流程

其他合作物件則依賴宣告的值來為 \- 其所裝載的 Web 應用程式執行授權工作。 在 AD FS 管理] 嵌入式管理單元中，這些合作物件稱為「 *信賴* 憑證者」 \- 。 同盟服務負責在許多不同的當事人之間代理信任。 它是設計用來處理並流動受信任的宣告交換，從最初將宣告（也稱為 AD FS 管理嵌入式管理單元中的 *宣告提供者* ）來源的組織 \- 到信賴憑證者。 接著，信賴憑證者會使用這些宣告來做出授權決策。

使用此程序的宣告傳遞流程稱為「宣告管線」。 宣告在宣告管線中的傳遞流程有三個步驟：

1.  從宣告提供者接收的宣告會由宣告提供者信任上的接受轉換規則處理。 這些規則決定會接受來自宣告提供者的哪些宣告。

2.  接受轉換規則的輸出會用來做為發佈授權規則的輸入。 這些規則決定是否允許使用者存取信賴憑證者。

3.  接受轉換規則的輸出會用來做為發佈轉換規則的輸入。 這些規則決定將傳送到信賴憑證者的宣告。

如需詳細資訊，請參閱[宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)

### <a name="how-claims-are-issued"></a>宣告的發佈方式

當您撰寫宣告規則時，宣告規則的連入宣告來源是根據您正在撰寫宣告提供者信任規則或信賴憑證者信任規則而定。 當您撰寫宣告提供者信任的宣告規則時，連入宣告是從信任的宣告提供者傳送到 Federation Service 的宣告。 當您撰寫信賴憑證者信任的規則時，連入宣告是適用之宣告提供者信任之宣告規則所輸出的宣告。 如需有關連入宣告與連出宣告的詳細資訊，請參閱 [宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)與[宣告引擎的角色](The-Role-of-the-Claims-Engine.md)。

## <a name="what-are-claim-types"></a>什麼是宣告類型？

宣告類型提供宣告值的上下文資訊。 它通常是以統一資源識別項 URI 來 \( 表示 \) 。 AD FS 可以支援任何宣告類型，而且預設會使用下表中的宣告類型進行設定。

|名稱|描述|URI|
|--------|---------------|-------|
|電子 \- 郵寄地址|使用者的電子 \- 郵寄地址|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 \/ identity \/ 宣告 \/ emailaddress|
|名字|使用者的名字|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 \/ identity \/ 宣告 \/ givenname|
|名稱|使用者的唯一名稱|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 身分 \/ 識別 \/ 宣告 \/ 名稱|
|UPN|使用者的使用者主體名稱 \( UPN \)|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 身分 \/ 識別 \/ 宣告 \/ upn|
|一般名稱|使用者的一般名稱。|HTTP： \/ \/schemas.xmlSoap.org \/ 宣告 \/ CommonName|
|AD FS 1.x 電子 \- 郵寄地址|\-與 AD FS 1.1 或 ADFS 1.0 交互操作時，使用者的電子郵件地址|HTTP： \/ \/schemas.xmlSoap.org \/ 宣告 \/ EmailAddress|
|群組|使用者所屬的群組|HTTP： \/ \/schemas.xmlSoap.org \/ 宣告 \/ 群組|
|AD FS 1.x UPN|與 AD FS 1.1 或 ADFS 1.0 搭配使用時的使用者 UPN|HTTP： \/ \/schemas.xmlSoap.org \/ 宣告 \/ UPN|
|角色|使用者擁有的角色|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ 角色|
|Surname|使用者的姓氏|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 \/ identity \/ 宣告 \/ 姓氏|
|PPID|使用者的私人識別碼|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 \/ identity \/ 宣告 \/ privatepersonalidentifier|
|名稱識別碼|使用者的 SAML 名稱識別碼|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 \/ identity \/ 宣告 \/ nameidentifier|
|驗證方法|用來驗證使用者的方法|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ authenticationmethod|
|僅拒絕群組 SID|使用者的 \- 僅限拒絕群組 SID|HTTP： \/ \/schemas.xmlsoap.org \/ ws \/ 2005 \/ 05 \/ identity \/ 宣告 \/ denyonlysid|
|僅拒絕主要 SID|使用者的 \- 僅限拒絕主要 SID|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ denyonlyprimarysid|
|僅拒絕主要群組 SID|使用者的 \- 僅限拒絕主要群組 SID|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ denyonlyprimarygroupsid|
|群組 SID|使用者的群組 SID|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ groupsid|
|主要群組 SID|使用者的主要群組 SID|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ primarygroupsid|
|主要 SID|使用者的主要 SID|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ primarysid|
|Windows 帳戶名稱|使用者的網域帳戶名稱，格式為 \<domain\>\\\<user\>|HTTP： \/ \/ schemas.microsoft.com \/ ws \/ 2008 \/ 06 身分 \/ 識別 \/ 宣告 \/ windowsaccountname|

## <a name="what-are-claim-descriptions"></a>什麼是宣告描述？

宣告描述代表 AD FS 支援且可在同盟中繼資料中發行的宣告類型清單。 上表中所述的宣告類型會設定為 AD FS 管理嵌入式管理單元中的宣告描述 \- 。

將發行到同盟中繼資料的宣告描述集合是儲存在 AD FS 設定資料庫中。 這些宣告描述會由 Federation Service 的各元件使用。

每個宣告描述都包含宣告類型 URI、名稱、發行狀態與描述。 您可以使用 AD FS 管理] 嵌入式管理單元中的 [宣告 **描述** ] 節點來管理宣告描述集合 \- 。 您可以使用嵌入式管理單元來修改宣告描述的發行狀態 \- 。 可用的設定如下：

- **在同盟中繼資料中，將此宣告發布為此同盟服務可接受** \( 的宣告類型[發佈為已接受] \) -表示此同盟服務將從其他宣告提供者接受的宣告類型。

- **在同盟中繼資料中發佈此宣告，作為此同盟服務可以傳送** \( 的宣告類型[發佈為已傳送] \) -表示此同盟服務所提供的宣告類型。 這些是 Federation Service 發行至其他宣告提供者且其他宣告提供者願意傳送的宣告類型。 此宣告提供者傳送的實際宣告類型通常是此清單的子集。

如需有關如何設定宣告類型之發行狀態的詳細資訊，請參閱 AD FS 部署指南中的 [新增宣告描述](../operations/add-a-claim-description.md) 。

### <a name="when-generating-federation-metadata"></a>同盟中繼資料產生時機

同盟中繼資料包含標示為待發行的所有宣告描述。

### <a name="when-claims-rules-are-processed"></a>宣告規則處理時機

當您保留宣告描述的相關設定資訊時，設定宣告相關規則將會比較簡單。 如需有關可在宣告提供者組織中使用之宣告規則的詳細資訊，請參閱[宣告規則的角色](The-Role-of-Claim-Rules.md)。
