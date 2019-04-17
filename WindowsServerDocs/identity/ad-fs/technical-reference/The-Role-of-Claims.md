---
ms.assetid: 22f53391-8c6a-4873-a1f4-08b4760ea621
title: "宣告的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98765deaba67ffdc0ee18b6d8ef573e531d739cc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-claims"></a>宣告的角色
在 claims\ 為基礎的身分型號，宣告聯盟過程中扮演關鍵的角色，他們的關鍵元件判斷的所有 Web\ 架構的驗證及授權要求的結果。 這種模式可讓組織確實投影數位身分和權利權限，或*宣告*，在 [安全性與企業範圍標準化的方式。  
  
## <a name="what-are-claims"></a>宣告為何？  
最簡單的形式在宣告只是*聲明*\（例如名稱、的身分，group\），對主要用於授權 claims\ 型位於網際網路上的任何位置的應用程式存取的使用者。 每個聲明對應至*值*，會儲存在理賠要求。  
  
### <a name="how-claims-are-sourced"></a>如何為宣告的來源  
在 Active Directory 同盟服務 \(AD FS\) 同盟服務定義所宣告的換貨之間聯盟。 不過，它可以執行此動作之前必須第一次填入或來源主張擷取或是計算的值。 每個理賠要求值代表使用者、群組中或實體的值，而為來源以兩種方式：  
  
1.  當構成理賠要求擷取值從屬性網上商店，例如屬性的值銷售部門擷取的 Active Directory 帳號屬性時。 如需詳細資訊，請查看[的屬性商店角色](The-Role-of-Attribute-Stores.md)。  
  
2.  當的價值，連入宣告會轉換成另一個值的邏輯以規則為基礎。 例如，連入時宣告網域系統管理員的值與會轉換成新的系統管理員的值做為傳出宣告傳送之前。 如需詳細資訊，請查看[的理賠要求規則角色](The-Role-of-Claim-Rules.md)。  
  
宣告可以包含例如 e\-電子郵件地址、使用者主體名稱 \(UPN\)、群組成員資格及其他 account 屬性的值。  
  
### <a name="how-claims-flow"></a>宣告 flow 的方式  
其他對象依賴索賠項目，才能執行這些裝載 Web\ 型應用程式授權工作值。 這些派對稱為*信賴派對*中 snap\ AD FS 管理。 聯盟服務負責仲介許多不同的對象之間信任。 它的設計目的是處理信任的交換宣告從一開始來源宣告，也稱為組織的方向，*宣告提供者*snap\ 單元，信賴 AD FS 管理。 信賴然後使用下列宣告做出授權。  
  
使用此程序宣告流程稱為*宣告管線*。 透過宣告管線宣告流程中有三個步驟：  
  
1.  宣告宣告提供者接收的處理方式主張提供者信任的接受轉換規則。 本規則判斷哪一個宣告接受宣告提供者。  
  
2.  接受轉換規則的輸出做為輸入發行授權規則。 本規則判斷使用者是否可以存取信賴。  
  
3.  接受轉換規則的輸出做為規則發行轉換輸入。 本規則判斷將被傳送至信賴主張。  
  
如需詳細資訊，請查看[宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)  
  
### <a name="how-claims-are-issued"></a>宣告發行的方式  
當您撰寫理賠要求規則時，連入宣告理賠要求規則的來源將視您是否信任宣告提供者或信賴的派對信任撰寫規則。 當您撰寫宣告提供者信任理賠要求規則時，連入宣告是宣告給受信任的宣告提供者同盟服務。 當您撰寫信賴的派對信任規則時，連入宣告是理賠要求規則的提供者適用宣告信任的輸出主張。 如需有關傳入宣告和傳出宣告，請查看[宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)和[角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。  
  
## <a name="what-are-claim-types"></a>有哪些類型理賠要求？  
宣告類型提供宣告值操作。 這通常表示統一資源識別碼 \(URI\) 為。 AD FS 可支援任何宣告類型，且宣告類型下表中的預設設定。  
  
|名稱|描述|URI|  
|--------|---------------|-------|  
|E\-電子郵件地址|使用者 e\-電子郵件地址|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress|  
|名字|指定的使用者名稱|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/givenname|  
|名稱|獨特的使用者名稱|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/name|  
|UPN|使用者主體名稱 \(UPN\) 的使用者|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/upn|  
|一般的名稱|一般的使用者名稱|http:///\/schemas.xmlsoap.org\/claims\/CommonName|  
|AD FS 1.x E\-電子郵件地址|相互操作 AD FS 1.1 或 ADFS 1.0 時的使用者 e\-電子郵件地址|http:///\/schemas.xmlsoap.org\/claims\/EmailAddress|  
|群組|群組的使用者的成員|http:///\/schemas.xmlsoap.org\/claims\/Group|  
|AD FS 1.x UPN|UPN 的相互操作 AD FS 1.1 或 ADFS 1.0 時的使用者|http:///\/schemas.xmlsoap.org\/claims\/UPN|  
|角色|使用者的角色|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/role|  
|姓氏|姓氏的使用者|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/surname|  
|PPID|使用者私人識別碼|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/privatepersonalidentifier|  
|名稱 Identifier|SAML 的使用者名稱 identifier|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/nameidentifier|  
|驗證方法|用來驗證使用者的方法|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/authenticationmethod|  
|拒絕僅限群組 SID|僅限 deny\ SID 使用者群組|http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/denyonlysid|  
|拒絕只主要 SID|僅限 deny\ 使用者主要 SID|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarysid|  
|拒絕主要群組 SID|僅限 deny\ 主要 SID 使用者群組|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/denyonlyprimarygroupsid|  
|SID 群組|使用者的 SID 群組|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/groupsid|  
|主要群組 SID|主要群組 SID 的使用者|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarygroupsid|  
|主要 SID|使用者的主要 SID|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/primarysid|  
|Windows account 名稱|網域 account 的形式的使用者名稱<domain>\\<user>|http:///\/schemas.microsoft.com\/ws\/2008\/06\/identity\/claims\/windowsaccountname|  
  
## <a name="what-are-claim-descriptions"></a>有哪些描述理賠要求？  
宣告描述代表宣告類型 AD FS 支援，且可能發行聯盟中繼資料中的清單。 一個表格中所提到宣告類型被設定為 snap\ 中 AD FS 管理宣告描述。  
  
AD FS 設定資料庫中儲存的理賠要求描述，將會發行至聯盟中繼資料的收藏。 這些主張使用各種不同的元件同盟服務的描述。  
  
每個宣告描述包含宣告類型 URI、名稱、發行狀態，以及描述。 您可以使用管理宣告描述收集**取得描述**節點 snap\ 中 AD FS 管理。 您可以修改發行宣告描述 snap\ 中使用的狀態。 可使用下列設定：  
  
-   **將此宣告聯盟中繼資料中發行為可接受此同盟服務理賠要求輸入**\(Publish as Accepted\)-表示將接受從其他宣告提供者，此同盟服務宣告類型。  
  
-   **將此宣告聯盟中繼資料中發行為此同盟服務可以傳送理賠要求輸入**\(Publish as Sent\)-表示提供此同盟服務宣告類型。 這些是同盟服務是願意傳送的發行給其他人宣告類型。 宣告提供者所傳送的實際理賠要求類型通常子集這份清單。  
  
如需有關如何將發行狀態宣告類型的設定，請查看[需要新增描述取得](https://technet.microsoft.com/library/dd807051.aspx)中的 AD FS 部署。  
  
### <a name="when-generating-federation-metadata"></a>當產生聯盟中繼資料  
聯盟中繼資料包含標示為發行的所有理賠要求描述。  
  
### <a name="when-claims-rules-are-processed"></a>當處理宣告規則  
當您保留宣告描述設定的資訊時，是讓您設定規則宣告有關更容易。 如需理賠要求規則可用宣告提供者組織中相關資訊，[的角色的取得規則](The-Role-of-Claim-Rules.md)。  
  

