---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: "設定密碼到期宣告傳送給 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 386a5ac921ba609c371121b8657351667628951b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>設定密碼到期宣告傳送給 AD FS

>適用於：Windows Server 2016、Windows Server 2012 R2

您可以設定傳送到信賴廠商信任（應用程式）所保護 ADFS 密碼到期宣告的 Active Directory 同盟 Services (AD FS)。 如何使用這些宣告應用程式而有所不同。 例如使用 Office 365，為您信賴，更新已實作通知聯盟的使用者他們即將-到--已過期的密碼換貨及 Outlook。

若要設定密碼傳送給 AD FS 到期宣告信賴的派對信任，您必須將下列理賠要求規則加入派對信任信賴：

```
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
=> issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> 密碼到期宣告只適用於的使用者名稱與密碼，以及 Microsoft Passport 工作驗證類型。  如果要驗證使用者使用 Windows 整合式的驗證和 Passport 未設定宣告將無法使用，使用者將不會看到密碼到期通知。

> [!NOTE]
> 會在 14 天，如果密碼在 14 天後過期，只會填入傳送的主張。

## <a name="see-also"></a>也了
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md)
