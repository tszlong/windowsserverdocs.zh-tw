---
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: 決定要使用的宣告規則範本類型
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1d38e23d7f1671f729b03b7e6f8000471d2e9f9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188655"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>決定要使用的宣告規則範本類型


設計 Active Directory 同盟服務很重要的一部分\(AD FS\)基礎結構會判斷一組完整的宣告規則 — 和哪個對應的宣告規則範本，您應該用來建立它們，為每個夥伴將會參與向您的組織的同盟。 您在 AD FS 管理嵌入式管理單元中使用宣告規則範本建立規則\-中。  
  
您設定的每一組宣告規則只能與一個同盟信任相關聯。 這表示，您無法在建立一個信任的一組規則後，將其用於同盟服務中的其他信任。 不過您可以從宣告規則範本輕鬆建立規則，以更快速地產生每個同盟夥伴與您的組織之間同意的一組宣告。  
  
如需有關規則和規則範本的詳細資訊，請參閱 [The Role of Claim Rules](The-Role-of-Claim-Rules.md)。  
  
在您決定所應使用的宣告規則範本類型之前，請考量下列問題：  
  
-   您信任的宣告提供者會提供哪些宣告？  
  
-   您信任每個宣告提供者的哪些宣告？  
  
-   信任此同盟服務的信賴憑證者需要哪些宣告？  
  
-   您願意向每個信賴憑證者揭露哪些宣告？  
  
-   哪些使用者應該可以存取每個信賴憑證者？  
  
回答這些問題將可協助您規劃完整的宣告規則設計。 這也有助於您建立妥善的授權和存取控制策略，並使部署團隊在實作期間更有效率。  
  
在下一節中，您將了解如何根據您的商業需求為您的環境選取適當的規則範本類型。  
  
## <a name="claim-rule-template-types"></a>宣告規則範本類型  
下表描述所有類型的宣告規則範本可供您使用 AD FS 管理嵌入式管理單元來建立規則\-在中，並透過另一個使用一種範本類型的優點。  
  
|規則範本類型|描述|優點|缺點|  
|----------------------|---------------|--------------|-----------------|  
|傳遞或篩選連入宣告|用來建立一個規則，此規則會傳遞所選宣告類型的所有宣告值，或根據宣告值篩選宣告，而只讓所選宣告類型的特定宣告值傳遞。<br /><br />如需詳細資訊，請參閱 [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)。|-可用來選取要接受或發出的特定宣告不變|宣告不能變更類型和值|  
|轉換傳入宣告|用來建立一個規則，此規則可選取傳入宣告，並將其對應至不同的宣告類型，或將其宣告值對應至新的宣告值。<br /><br />如需詳細資訊，請參閱 [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md)。|-可以用來標準化宣告類型或值<br />-可以取代電子\-郵件後置詞的傳入宣告|-更複雜的字串取代需要自訂規則|  
|以宣告形式傳送 LDAP 屬性|用來建立一個規則，從 LDAP 屬性存放區中選取屬性，以宣告的形式傳送給信賴憑證者。<br /><br />如需詳細資訊，請參閱 [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)。|-可以來源從任何 AD DS 宣告\/AD LDS 屬性存放區<br />-多個宣告可以使用單一規則發行|-效能 – 因帳戶查閱而變慢<br />-無法使用自訂 LDAP 篩選器進行查詢|  
|以宣告的形式傳送群組成員資格|用來建立一個規則，此規則可在使用者是 Active Directory 安全性群組的成員時傳送指定的宣告類型和值。 使用這項規則時，會根據您選取的群組傳送單一宣告。<br /><br />如需詳細資訊，請參閱 [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。|-發行群組宣告 – 不進行帳戶查閱的效能快|使用者必須是本機的 Active Directory 群組的成員|  
|使用自訂規則傳送宣告|用來建立會比標準規則範本提供更進階之選項的自訂規則。 您撰寫自訂的規則與 AD FS 宣告規則語言。<br /><br />如需詳細資訊，請參閱 [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md)。|-可以用來從 SQL 屬性存放區尋找宣告來源<br />-可以用來指定自訂 LDAP 篩選器<br />-可以用來發行 PPID<br />-可以搭配自訂屬性存放區<br />-可以用來將宣告新增至輸入的宣告集僅<br />-可用來根據一個以上的傳入宣告傳送宣告|-更難設定\-準備時間可能會需要用到一開始瞭解宣告規則語言|  
|根據連入宣告允許或拒絕使用者|用來建立一個規則，此規則會根據傳入宣告的類型和值，允許或拒絕使用者對信賴憑證者的存取。<br /><br />如需詳細資訊，請參閱 [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md)。|-簡化授權程序|-需要只有一個宣告類型和一個宣告值會指定<br />-不支援的模式比對宣告值|  
|允許所有使用者|用來建立將允許所有使用者存取信賴憑證者的規則。<br /><br />如需詳細資訊，請參閱 [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md)。|-簡單設定|-安全性不如使用允許或拒絕使用者根據傳入宣告的範本|  
  

