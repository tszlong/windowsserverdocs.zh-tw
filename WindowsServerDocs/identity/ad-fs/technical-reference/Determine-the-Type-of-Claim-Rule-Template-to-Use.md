---
description: 深入瞭解：判斷要使用的宣告規則範本類型
ms.assetid: 696a29b2-d627-4c9a-a384-9c8aaf50bd23
title: 決定要使用的宣告規則範本類型
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a3609ee6676f2db2ad3007509bbd81f70d68014a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042386"
---
# <a name="determine-the-type-of-claim-rule-template-to-use"></a>決定要使用的宣告規則範本類型


設計 Active Directory 同盟服務 AD FS 基礎結構的其中一個重要部分， \( \) 就是決定一組完整的宣告規則，以及您應該用來建立它們的對應宣告規則範本，適用于參與您組織同盟的每個合作夥伴。 您可以使用 [AD FS 管理] 嵌入式管理單元中的 [宣告規則範本] 來建立規則 \- 。

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
下表說明您可以使用 AD FS 管理嵌入式管理單元來建立規則的所有宣告規則範本類型 \- ，以及在另一個範本類型之間使用的優點。

|規則範本類型|描述|優點|缺點|
|----------------------|---------------|--------------|-----------------|
|傳遞或篩選連入宣告|用來建立一個規則，此規則會傳遞所選宣告類型的所有宣告值，或根據宣告值篩選宣告，而只讓所選宣告類型的特定宣告值傳遞。<p>如需詳細資訊，請參閱 [When to Use a Pass Through or Filter Claim Rule](When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)。|-可以用來選取要接受或未變更的特定宣告|-無法變更宣告類型和值|
|傳輸傳入宣告|用來建立一個規則，此規則可選取傳入宣告，並將其對應至不同的宣告類型，或將其宣告值對應至新的宣告值。<p>如需詳細資訊，請參閱 [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md)。|-可以用來將宣告類型或值標準化<br />-可以取代連 \- 入宣告的電子郵件尾碼|-更複雜的字串取代需要自訂規則|
|以宣告形式傳送 LDAP 屬性|用來建立一個規則，從 LDAP 屬性存放區中選取屬性，以宣告的形式傳送給信賴憑證者。<p>如需詳細資訊，請參閱 [When to Use a Send LDAP Attributes as Claims Rule](When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)。|-可以從任何 AD DS \/ AD LDS 屬性存放區宣告來源<br />-可以使用單一規則來發行多個宣告|-效能–因為帳戶查閱結果變慢<br />-無法使用自訂 LDAP 篩選器進行查詢|
|以宣告的形式傳送群組成員資格|用來建立一個規則，此規則可在使用者是 Active Directory 安全性群組的成員時傳送指定的宣告類型和值。 使用這項規則時，會根據您選取的群組傳送單一宣告。<p>如需詳細資訊，請參閱 [When to Use a Send Group Membership as a Claim Rule](When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)。|-發出群組宣告的快速效能–無帳戶查閱|-使用者必須是本機 Active Directory 群組的成員|
|使用自訂規則傳送宣告|用來建立會比標準規則範本提供更進階之選項的自訂規則。 您可以使用 AD FS 宣告規則語言來撰寫自訂規則。<p>如需詳細資訊，請參閱 [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md)。|-可用於來自 SQL 屬性存放區的來源宣告<br />-可以用來指定自訂 LDAP 篩選器<br />-可以用來發出 PPID<br />-可搭配自訂屬性存放區使用<br />-只能用來將宣告新增至輸入宣告集<br />-可用來根據一個以上的連入宣告來傳送宣告|-若要設定 \- 一些加速時間，您可能需要一開始就取得宣告規則語言的知識|
|根據連入宣告允許或拒絕使用者|用來建立一個規則，此規則會根據傳入宣告的類型和值，允許或拒絕使用者對信賴憑證者的存取。<p>如需詳細資訊，請參閱 [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md)。|-簡化授權程式|-只需要指定一個宣告類型和一個宣告值<br />-不支援宣告值的模式比對|
|允許所有使用者|用來建立將允許所有使用者存取信賴憑證者的規則。<p>如需詳細資訊，請參閱 [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md)。|-簡單的設定|-相較于根據連入宣告範本使用允許或拒絕使用者的安全|


