---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: 使用自訂宣告規則的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b08622cd9eefa153e2fe8403fbe85077644182f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813109"
---
>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>使用自訂宣告規則的時機
您可以撰寫自訂宣告規則中 Active Directory Federation Services \(AD FS\)使用宣告規則語言，這是宣告發行引擎用來以程式設計方式產生、 轉換、 通過和篩選宣告。 藉由使用自訂規則，您可以透過比標準規則範本更複雜的邏輯來建立規則。 當您想要執行下列作業時，請考慮使用自訂規則：  
  
-   根據從結構化的查詢語言擷取的值來傳送宣告\(SQL\)屬性存放區。  
  
-   根據從輕量型目錄存取通訊協定擷取的值來傳送宣告\(LDAP\)使用自訂 LDAP 篩選器的屬性存放區。  
  
-   根據從自訂屬性存放區中擷取的值來傳送宣告。  
  
-   僅在有兩個或更多傳入宣告存在時傳送宣告。  
  
-   僅在傳入宣告值符合複雜模式時傳送宣告。  
  
-   在傳入宣告值有複雜的變更時傳送宣告。  
  
-   建立僅用於後續規則的宣告，而不實際傳送宣告。  
  
-   從多個連入宣告的內容建構傳出宣告。  
  
如果傳出宣告的宣告值必須以傳入宣告的值為準，但還必須包含其他內容，您也可以使用自訂規則。  
  
宣告規則語言以規則為準。 它有條件組件和執行組件。 您可以使用宣告規則語言語法來列舉、新增、刪除或修改宣告，以符合組織的需求。 如需有關如何每一種組件運作方式的詳細資訊，請參閱 < [The Role of 宣告規則語言](The-Role-of-the-Claim-Rule-Language.md)。  
  
下列各節提供宣告規則的基本介紹。 此外也提供關於如何使用自訂宣告規則的詳細資料。  
  
## <a name="about-claim-rules"></a>關於宣告規則  
宣告規則表示商務邏輯接受連入宣告的執行個體，對其套用條件\(若 x 則 y\)和產生根據條件參數傳出宣告。  
  
> [!IMPORTANT]  
> -   在 AD FS 管理嵌入式管理單元\-，宣告可以只使用宣告規則範本建立規則  
> -   宣告規則處理程序內送宣告直接從宣告提供者\(Active Directory 或其他同盟服務等\)或宣告提供者信任上從輸出的接受轉換規則。  
> -   宣告規則的處理方式，是由宣告發行引擎依時間先後順序按照指定的規則集處理。 藉由設定規則優先順序，您可以進一步精簡或篩選由特定規則集內的上一個規則所產生的宣告。  
> -   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則和同一宣告類型處理多個宣告值。  
  
如需詳細的宣告規則和宣告規則集的相關資訊，請參閱[規則的角色宣告](The-Role-of-Claim-Rules.md)。 如需有關規則的處理方式的詳細資訊，請參閱 < [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 宣告規則集的處理方式的詳細資訊，請參閱[The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="how-to-create-this-rule"></a>如何建立此規則  
建立此規則的第一個撰寫的語法，您需要您使用宣告規則語言，然後貼上的項目，在文字中的結果也就是方塊的作業提供在傳送的宣告，使用自訂規則範本內容的宣告提供者 trust 或信賴憑證者信任 AD FS 管理嵌入式管理單元中\-中。  
  
此規則範本提供以下選項：  
  
-   指定宣告規則名稱  
  
-   輸入一或多個選用條件和發佈陳述式使用 AD FS 宣告規則語言  
  
建立自訂規則使用此範本的詳細指示，請參閱[建立自訂規則傳送宣告使用的規則](https://technet.microsoft.com/library/dd807049.aspx)AD FS 部署指南中。  
  
深入了解宣告規則語言的運作方式，檢視已存在其他規則的宣告規則語言語法中的嵌入式管理單元\-中按一下**檢視規則語言**該規則屬性 索引標籤。 使用本節的資訊和此索引標籤上的語法資訊，可深入了解如何建構您自己的自訂規則。  
  
如需如何使用宣告規則語言的詳細資訊，請參閱[The Role of 宣告規則語言](The-Role-of-the-Claim-Rule-Language.md)。  
  
## <a name="using-the-claim-rule-language"></a>使用宣告規則語言  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>範例：如何根據使用者的名稱屬性值結合名字和姓氏  
下列規則語法會結合會從給定屬性存放區中的屬性值結合名字和姓氏。 原則引擎會組成每個條件之相符項目的笛卡兒乘積。 例如，名字 {“Frank”, “Alan”} 和姓氏 {“Miller”, “Shen”} 的輸出是 {“Frank Miller”, “Frank Shen”, “Alan Miller”, “Alan Shen”}：  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>範例：如何根據使用者是否有直接報告發出管理員宣告  
下列規則只會在使用者有直接報告時發出管理員宣告：  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>範例：如何根據 LDAP 屬性發出 PPID 宣告  
下列規則發出私密個人識別碼\(PPID\)宣告根據**windowsaccountname**並**originalissuer** LDAP 屬性中的使用者屬性存放區：  
  
```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
可用來唯一識別此查詢之使用者的常用屬性包括：  
  
-   **使用者 SID**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

