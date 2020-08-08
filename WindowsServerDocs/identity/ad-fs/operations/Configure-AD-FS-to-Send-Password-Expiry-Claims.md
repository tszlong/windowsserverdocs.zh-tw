---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: 設定 AD FS 傳送密碼到期宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e53c33f1fef30570db46cf658bd1481343e846cd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962542"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>設定 AD FS 傳送密碼到期宣告


您可以設定 Active Directory 同盟服務 (AD FS) ，將密碼到期宣告傳送給受 ADFS 保護 (應用程式) 的信賴憑證者信任。 這些宣告的使用方式取決於應用程式。 例如，若以 Office 365 作為您的信賴憑證者，Exchange 和 Outlook 已實作了相關更新，會在同盟使用者的密碼即將過期時對其發出通知。

若要設定 AD FS 將密碼到期宣告傳送至信賴憑證者信任，您必須在此信賴憑證者信任中新增下列宣告規則：

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> 密碼到期宣告僅適用于使用者名稱和密碼，以及 Microsoft Passport for Work 驗證類型。  如果使用者使用「Windows 整合式驗證」進行驗證，且未設定 Passport，則宣告將無法使用，且使用者不會看到密碼到期通知。

> [!NOTE]
> 有14天的時間範圍，只有在密碼于14天內到期時，才會填入已傳送的宣告。

## <a name="see-also"></a>另請參閱
[AD FS 操作](../ad-fs-operations.md)
