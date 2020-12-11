---
description: 深入瞭解：設定 AD FS 以傳送密碼到期宣告
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: 設定 AD FS 傳送密碼到期宣告
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d043f86486d0f3370a310ec12cf1d0dfd937795b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046706"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>設定 AD FS 傳送密碼到期宣告


您可以設定 Active Directory 同盟服務 (AD FS) ，將密碼到期宣告傳送至信賴憑證者信任 (受 ADFS 保護的應用程式) 。 這些宣告的使用方式取決於應用程式。 例如，若以 Office 365 作為您的信賴憑證者，Exchange 和 Outlook 已實作了相關更新，會在同盟使用者的密碼即將過期時對其發出通知。

若要將 AD FS 設定為將密碼到期宣告傳送給信賴憑證者信任，您必須將下列宣告規則新增至這個信賴憑證者信任：

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> 密碼到期宣告僅適用于使用者名稱和密碼，以及 Microsoft Passport for Work 驗證類型。  如果使用者使用 Windows 整合式驗證進行驗證，且未設定 Passport，則將無法使用宣告，使用者將不會看到密碼到期通知。

> [!NOTE]
> 有14天的時間範圍，因此只有在密碼在14天內到期時，才會填入傳送的宣告。

## <a name="see-also"></a>另請參閱
[AD FS 操作](../ad-fs-operations.md)
