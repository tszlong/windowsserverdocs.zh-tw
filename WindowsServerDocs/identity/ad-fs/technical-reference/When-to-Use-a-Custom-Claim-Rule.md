---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: "使用 [自訂理賠要求規則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5398f0b10d9e62548145fdde0354a3d047eb0ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>使用 [自訂理賠要求規則
您在使用理賠要求規則語言、 架構宣告發行引擎使用以程式設計方式產生轉換、 通過會 Active Directory 同盟服務 \(AD FS\) 撰寫自訂理賠要求規則及篩選與主張。 使用自訂規則，您可以建立具有更複雜的邏輯比一般規則範本規則。 請考慮當您想要使用自訂的規則：  
  
-   傳送宣告基礎結構化查詢語言 \(SQL\) 屬性存放區擷取的值。  
  
-   傳送主張使用自訂 LDAP 篩選輕量型 Directory 存取通訊協定 \(LDAP\) 屬性存放區擷取的值為基礎。  
  
-   傳送宣告根據自訂屬性存放區擷取的值。  
  
-   有兩個或更多連入宣告時才，請傳送主張。  
  
-   傳送宣告，連入取得值相符項目複雜的模式時，才。  
  
-   傳送到傳入複雜變更宣告取得值。  
  
-   只會在稍後規則，建立主張使用，而不需要實際傳送宣告。  
  
-   建立的多個連入宣告 content 從傳出宣告。  
  
傳出宣告宣告值必須為基礎的值連入宣告，但必須也包含其他 content 時，您也可以使用 [自訂規則。  
  
宣告規則語言是根據規則。 它已條件和執行部分。 您可以使用理賠要求規則語言語法列舉、 新增、 delete，或修改宣告貴組織的需求。 針對每個部分運作方式的相關詳細資訊，請查看[的角色理賠要求規則語言的](The-Role-of-the-Claim-Rule-Language.md)。  
  
下列章節提供基本簡介取得規則。 它們也提供使用自訂宣告規則詳細資訊。  
  
## <a name="about-claim-rules"></a>關於理賠要求規則  
宣告規則表示商務邏輯操作，連入宣告的執行個體、 適用於條件 \ （如果 x，然後 y\） 和產生傳出宣告依據條件的參數。  
  
> [!IMPORTANT]  
> -   AD FS snap\ 中管理，取得可以只使用理賠要求規則範本建立規則  
> -   宣告規則處理程序傳入宣告直接從宣告提供者 \ （例如 Active Directory 或其他聯盟 Service\） 或接受的輸出從轉換宣告提供者信任規則。  
> -   宣告發行引擎順序特定的規則集中處理理賠要求規則。 藉由設定優先順序規則，可以進一步改善或篩選宣告專特定的規則設定中的上一個規則。  
> -   宣告規則範本一定需要您指定傳入宣告類型。 不過，您可以使用相同的理賠要求類型處理多個理賠要求值使用單一規則。  
  
如需詳細資訊理賠要求規則及宣告規則集合，請查看[的角色的取得規則](The-Role-of-Claim-Rules.md)。 如需規則的處理方式的相關資訊，請查看[的角色宣告引擎的](The-Role-of-the-Claims-Engine.md)。 宣告規則集合的處理方式的相關資訊，請查看[的角色宣告管線的](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="how-to-create-this-rule"></a>如何建立本規則  
您第一次製作，您必須使用理賠要求規則語言作業然後結果貼上所提供的傳送文字方塊主張使用宣告提供者的屬性自訂規則範本信任或信賴信任 snap\ 中 AD FS 管理語法建立此規則。  
  
此規則範本提供下列選項：  
  
-   指定名稱理賠要求規則  
  
-   輸入一個或多個選擇性 and 發行聲明，請使用 AD FS 取得規則語言  
  
建立自訂使用此範本規則的其他的指示，請查看[建立自訂規則傳送主張使用規則](https://technet.microsoft.com/library/dd807049.aspx)中的 AD FS 部署。  
  
解理賠要求規則語言的運作方式，檢視理賠要求規則語言的其他存在的規則語法 snap\ 中按一下**檢視規則語言**索引標籤中，規則的屬性。 使用此一節中的資訊和語法資訊此索引標籤上，可提供深入了解如何建立您自己的自訂規則。  
  
如需有關如何使用理賠要求規則語言，請查看[角色取得規則語言的](The-Role-of-the-Claim-Rule-Language.md)。  
  
## <a name="using-the-claim-rule-language"></a>使用語言理賠要求規則  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>範例： 如何結合姓名根據的使用者名稱屬性的值  
下列語法規則結合了特定地區姓名中指定的屬性網上商店的屬性。 原則引擎形成笛 product 的每個條件相符項目。 例如，{」 Frank 」、 「 心靈 「} 名字與姓氏 {「 李玉紅 」、 「 Shen 「} 輸出為 {「 Frank 李玉紅 」、 「 Frank Shen 」、 「 心靈李玉紅 」、 「 心靈 Shen 「}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>範例： 如何發出根據使用者是否有屬下管理員理賠要求  
下列規則問題管理員理賠要求只有使用者有直接報告：  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>範例： 如何發出根據 LDAP 屬性 PPID 理賠要求  
下列規則問題私人個人識別碼 \(PPID\) 理賠要求根據**windowsaccountname**和**originalissuer**屬性 LDAP 屬性網上商店中的使用者：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
常見的屬性，可用於唯一找出使用者提供這項查詢包含下列類型：  
  
-   **使用者 SID**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

