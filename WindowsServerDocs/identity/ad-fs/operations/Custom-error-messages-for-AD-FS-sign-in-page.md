---
description: 深入瞭解： AD FS 登入頁面的自訂錯誤訊息
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: AD FS 登入頁面的自訂錯誤訊息
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: acfe34a7d151a7e14348e44350e9304a816428b9
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039906"
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>AD FS 登入頁面的自訂錯誤訊息

您可以設定自訂錯誤訊息，以符合組織需求。 下圖顯示自訂的錯誤頁面描述，以及一般錯誤訊息。 使用下列 Windows PowerShell Cmdlet 自訂錯誤訊息。

![自訂錯誤](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)

## <a name="customize-the-error-page-description"></a>自訂錯誤頁面描述

若要自訂錯誤頁面描述，請使用下列 Windows PowerShell Cmdlet 和語法。

```powershell
Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description"
```

## <a name="customize-a-generic-error-message"></a>自訂一般錯誤訊息
若要自訂一般錯誤訊息，請使用下列 Windows PowerShell Cmdlet 和語法。

```powershell
Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance."
```

## <a name="customize-an-authorization-error-message"></a>自訂授權錯誤訊息
若要自訂授權錯誤訊息，請使用下列 Windows PowerShell Cmdlet 和語法。

```powershell
Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."
```

## <a name="customize-a-device-authentication-error-message"></a>自訂裝置驗證錯誤訊息
若要自訂裝置驗證錯誤訊息，請使用下列 Windows PowerShell Cmdlet 和語法。

```powershell
Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."
```

## <a name="customize-a-support-email-error-message"></a>自訂支援電子郵件錯誤訊息
您可以在 AD FS 中設定支援電子郵件地址。 若已設定，AD FS 會自動顯示連結，讓使用者以電子郵件傳送錯誤詳細資料。

若要自訂支援電子郵件錯誤訊息，請使用下列 Windows PowerShell Cmdlet 和語法。

```powershell
Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"
```

## <a name="customize-a-relying-party-authorization-message"></a>自訂信賴憑證者授權訊息
您可以在 AD FS 中設定信賴憑證者授權錯誤訊息。

若要自訂信賴憑證者錯誤訊息，請使用下列 Windows PowerShell Cmdlet 和語法。

```powershell
Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>"
```

## <a name="additional-references"></a>其他參考資料

[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
