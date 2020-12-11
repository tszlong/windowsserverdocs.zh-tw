---
description: 深入瞭解：何時使用傳送群組成員資格作為宣告規則
ms.assetid: af16e847-47c2-461e-9df1-cc352a322043
title: 使用「以宣告方式傳送群組成員資格」規則的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 912ebf3fe8c3db96de2d615d3bda6ea3c7f3f7a4
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050426"
---
# <a name="when-to-use-a-send-group-membership-as-a-claim-rule"></a>使用「以宣告方式傳送群組成員資格」規則的時機
\( \) 當您只想針對屬於指定之 Active Directory 安全性群組成員的使用者發出新的外寄宣告值時，您可以在 Active Directory 同盟服務 AD FS 中使用此規則。 使用這項規則時，您只需針對您指定且符合規則邏輯的群組發出單一宣告，如下表所述。

|規則選項|規則邏輯|
|---------------|--------------|
|傳出宣告值|如果使用者的群組成員資格等於 *指定的群組* ，且連出的宣告類型等於指定的宣告 *類型*，請將現有的組名值取代為指定的 *傳出宣告值* ，併發出宣告。|

下列各節提供宣告規則的基本介紹。 另還針對使用「以宣告方式傳送群組成員資格」規則的時機提供詳細資料。

## <a name="about-claim-rules"></a>關於宣告規則
宣告規則代表將採用傳入宣告之商務邏輯的實例，如果 x then y 則將條件套用至該實例， \( \) 並根據條件參數產生外送宣告。 在您進一步閱讀本主題之前，請先閱讀下列清單，其中會概述關於宣告規則您應該知道的重要秘訣：

-   在 AD FS 管理嵌入式管理單元 \- 中，只能使用宣告規則範本來建立宣告規則

-   宣告規則會直接從宣告提供者 \( （例如 Active Directory 或其他同盟服務， \) 或從宣告提供者信任的接受轉換規則輸出中處理傳入宣告。

-   宣告規則的處理方式，是由宣告發行引擎依時間先後順序按照指定的規則集處理。 藉由設定規則優先順序，您可以進一步精簡或篩選由特定規則集內的上一個規則所產生的宣告。

-   宣告規則範本一律會要求您指定傳入宣告類型。 不過，您可以使用單一規則和同一宣告類型處理多個宣告值。

如需宣告規則和宣告規則集的詳細資訊，請參閱宣告 [規則的角色](The-Role-of-Claim-Rules.md)。 如需有關如何處理規則的詳細資訊，請參閱 [宣告引擎的角色](The-Role-of-the-Claims-Engine.md)。 如需如何處理宣告規則集的詳細資訊，請參閱 [宣告管線的角色](The-Role-of-the-Claims-Pipeline.md)。

## <a name="outgoing-claim-value"></a>傳出宣告值
使用「以宣告方式傳送群組成員資格」規則範本，您可以發出視使用者是否為您指定群組成員而定的宣告。

換句話說，只有當使用者的群組安全識別碼 \( SID \) 符合系統管理員所指定的 Active Directory 群組時，此規則範本才會發出宣告。 針對 Active Directory Domain Services AD DS 進行驗證的所有使用者， \( \) 將會有每個所屬群組的傳入群組 SID 宣告。 根據預設，Active Directory 宣告提供者信任中的接受轉換規則會傳遞這些群組 SID 宣告。 使用這些群組 Sid 做為發出宣告的基礎，會比在 AD DS 中查閱使用者群組更快。

使用這項規則時，根據您選取的 Active Directory 群組，只會傳送一個宣告。 比方說，您可以使用這個規則範本建立以下規則：如果使用者是 Domain Admins 安全性群組的成員，則會傳送值為 "Admin" 的群組宣告。

## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>在宣告提供者信任上設定此規則
只有當從宣告提供者收到群組 SID 時 (這個情況對任何宣告提供者而言並不常見，Active Directory 或 AD DS 除外)，系統管理員應該在宣告提供者信任的接受轉換規則中使用這個規則類型。

## <a name="how-to-create-this-rule"></a>如何建立此規則
您可以使用宣告規則語言來建立此規則，或在 AD FS 管理] 嵌入式管理單元中使用 [傳送 LDAP 群組成員資格作為宣告規則] 範本來建立此規則 \- 。 此規則範本提供以下設定選項：

-   指定宣告規則名稱

-   使用物件選擇器選取使用者的群組

-   選取傳出宣告類型

-   選取 [外寄名稱識別碼] 格式， \( 只有在從 [傳出宣告類型] 欄位中選擇名稱識別碼時才能使用\)

-   指定傳出宣告值

如需有關如何建立此規則的詳細資訊，請參閱 [建立規則以傳送群組成員資格作為](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913569(v=ws.11))宣告。

## <a name="using-the-claim-rule-language"></a>使用宣告規則語言
如果您想要根據傳入 SID (而不是群組 SID) 發行宣告，請使用「轉換傳入宣告」規則範本。 如果系統管理員想要擷取使用者為其成員的所有群組名稱，請使用「以宣告方式傳送 LDAP 屬性」規則範本，而不是使用 **tokenGroups** 屬性。

### <a name="example-how-to-issue-group-claims-based-on-the-users-group-membership"></a>範例：如何根據使用者的群組成員資格發出群組宣告
下列規則會根據連入群組 SID 為使用者發出群組宣告：

```
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-21-397933417-626991126-188441444-512", Issuer == "AD AUTHORITY"]
=> issue(Type = "http://schemas.xmlsoap.org/claims/Group", Value = "administrators", Issuer = c.Issuer, OriginalIssuer = c.OriginalIssuer, ValueType = c.ValueType);
```

## <a name="additional-references"></a>其他參考資料
[建立規則以傳送 LDAP 屬性作為宣告](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807115(v=ws.11))

