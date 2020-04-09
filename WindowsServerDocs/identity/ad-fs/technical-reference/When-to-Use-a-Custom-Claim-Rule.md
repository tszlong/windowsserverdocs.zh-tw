---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: 使用自訂宣告規則的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 41e7ea7c2bc627f2fce198e5c7227148e8b03d88
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853821"
---
# <a name="when-to-use-a-custom-claim-rule"></a>使用自訂宣告規則的時機
您可以使用宣告規則語言，在 Active Directory 同盟服務 \(AD FS\) 中撰寫自訂宣告規則，這是宣告發行引擎用來以程式設計方式產生、轉換、傳遞和篩選宣告的架構。 藉由使用自訂規則，您可以透過比標準規則範本更複雜的邏輯來建立規則。 當您想要執行下列作業時，請考慮使用自訂規則：  
  
-   根據從結構化查詢語言 (SQL) \(SQL\) 屬性存放區中解壓縮的值來傳送宣告。  
  
-   使用自訂 LDAP 篩選器 \(LDAP\) 屬性存放區，根據從輕量目錄存取通訊協定解壓縮的值來傳送宣告。  
  
-   根據從自訂屬性存放區中擷取的值來傳送宣告。  
  
-   僅在有兩個或更多傳入宣告存在時傳送宣告。  
  
-   僅在傳入宣告值符合複雜模式時傳送宣告。  
  
-   在傳入宣告值有複雜的變更時傳送宣告。  
  
-   建立僅用於後續規則的宣告，而不實際傳送宣告。  
  
-   從多個連入宣告的內容建構傳出宣告。  
  
如果傳出宣告的宣告值必須以傳入宣告的值為準，但還必須包含其他內容，您也可以使用自訂規則。  
  
宣告規則語言以規則為準。 它有條件組件和執行組件。 您可以使用宣告規則語言語法來列舉、新增、刪除或修改宣告，以符合組織的需求。 如需每個元件運作方式的詳細資訊，請參閱宣告[規則語言的角色](The-Role-of-the-Claim-Rule-Language.md)。  
  
下列各節提供宣告規則的基本介紹。 此外也提供關於如何使用自訂宣告規則的詳細資料。  
  
## <a name="about-claim-rules"></a>關於宣告規則  
宣告規則代表採用傳入宣告的商務邏輯實例、將條件套用至 \(如果 x，然後按 y\) 並根據條件參數產生傳出宣告。  
  
> [!IMPORTANT]  
> -   在的 [AD FS 管理] 嵌入式管理單元\-的中，只能使用宣告規則範本建立宣告規則  
> -   宣告規則會直接從宣告 \(提供者（例如 Active Directory 或其他同盟服務\)，或從宣告提供者信任的接受轉換規則輸出）處理傳入宣告。  
> -   宣告規則的處理方式，是由宣告發行引擎依時間先後順序按照指定的規則集處理。 藉由設定規則優先順序，您可以進一步精簡或篩選由特定規則集內的上一個規則所產生的宣告。  
> -   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則和同一宣告類型處理多個宣告值。  
  
如需宣告規則和宣告規則集的詳細資訊，請參閱宣告[規則的角色](The-Role-of-Claim-Rules.md)。 如需如何處理規則的詳細資訊，請參閱[宣告引擎的角色](The-Role-of-the-Claims-Engine.md)。 如需宣告規則集處理方式的詳細資訊，請參閱[宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="how-to-create-this-rule"></a>如何建立此規則  
若要建立此規則，您必須先使用宣告規則語言撰寫作業所需的語法，然後將結果貼入 [使用自訂規則來傳送宣告] 範本中的 [在中的 AD FS 管理] 嵌入式\-管理單元中，或信賴憑證者信任的內容中所提供的文字方塊。  
  
此規則範本提供以下選項：  
  
-   指定宣告規則名稱  
  
-   使用 AD FS 宣告規則語言輸入一或多個選擇性條件和發行語句  
  
如需使用此範本建立自訂規則的詳細指示，請參閱 AD FS 部署指南中的[使用自訂規則建立規則以傳送宣告](https://technet.microsoft.com/library/dd807049.aspx)。  
  
若要進一步瞭解宣告規則語言的運作方式，請按一下該規則內容中的 **查看規則語言** 索引標籤，以查看中已存在之 貼齊\-中的其他規則的宣告規則語言語法。 使用本節的資訊和此索引標籤上的語法資訊，可深入了解如何建構您自己的自訂規則。  
  
如需如何使用宣告規則語言的詳細資訊，請參閱宣告[規則語言的角色](The-Role-of-the-Claim-Rule-Language.md)。  
  
## <a name="using-the-claim-rule-language"></a>使用宣告規則語言  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>範例：如何根據使用者的名稱屬性值來合併名字和姓氏  
下列規則語法會結合會從給定屬性存放區中的屬性值結合名字和姓氏。 原則引擎會組成每個條件之相符項目的笛卡兒乘積。 例如，名字 {"Frank"，"Alan"} 和 last name {"莎莎"，"Shen"} 的輸出是 {"Frank 莎莎"，"Frank Shen"，"Alan 莎莎"，"Alan Shen"}：  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + "  " + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>範例：如何根據使用者是否有直接報告發出管理員宣告  
下列規則只會在使用者有直接報告時發出管理員宣告：  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == "http://schemas.xmlsoap.org/claims/Reports"] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>範例：如何根據 LDAP 屬性發出 PPID 宣告  
下列規則會根據 LDAP 屬性存放區中使用者的**windowsaccountname**和**originalissuer**屬性，發出私人個人識別碼 \(PPID\) 宣告：  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
可用來唯一識別此查詢之使用者的常用屬性包括：  
  
-   **使用者 SID**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

