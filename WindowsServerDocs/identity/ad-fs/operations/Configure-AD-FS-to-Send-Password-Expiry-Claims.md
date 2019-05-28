---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: 設定 AD FS 傳送密碼到期宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3be14b824038e9424b86c40bfd657dd988fa99e9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189875"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>設定 AD FS 傳送密碼到期宣告


您可以設定要傳送到信賴憑證者信任 （應用程式） 所保護的 ADFS 的密碼到期宣告的 Active Directory Federation Services (AD FS)。 如何使用這些宣告，則應用程式而定。 例如，搭配您信賴憑證者的合作對象為 Office 365，更新已實作 Exchange 和 Outlook，通知他們即將-至--已過期的密碼的同盟的使用者。

若要設定 AD FS 傳送密碼到期宣告給信賴憑證者信任，您必須新增下列宣告規則此信賴憑證者信任：

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> 密碼到期宣告只適用於使用者名稱和密碼與 Microsoft Passport for Work 的驗證類型。  如果使用者驗證未設定使用 Windows 整合式的驗證和 Passport，宣告將無法使用和使用者看不到密碼到期通知。

> [!NOTE]
> 沒有 14 天 視窗，所以如果密碼即將於 14 天內，才會填入已傳送的宣告。

## <a name="see-also"></a>另請參閱
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
