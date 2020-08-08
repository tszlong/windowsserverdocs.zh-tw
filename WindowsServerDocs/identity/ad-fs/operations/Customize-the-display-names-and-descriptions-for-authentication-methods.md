---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: 自訂驗證方法的顯示名稱和描述
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 679f60d1328795ef4f272f0159c6be5185428d59
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954264"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>自訂驗證方法的顯示名稱和描述

若要自訂驗證方法的顯示名稱和描述，您可以使用 `Set-AdfsAuthenticationProviderWebContent` PowerShell Cmdlt。  若要使用此 Cmdlt，您必須先取得您想要自訂的驗證方法的名稱。  作法是使用 `Get-AdfsGlobalAuthenticationPolicy`。  在下面的範例中，我們看到 \- [登入] 頁面上顯示下列內容：「使用 x.509 憑證登入」。  我們想要為使用者簡化這行文字。

![自訂 displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)

首先，我們取得驗證方法的名稱，然後編輯顯示的文字。

```powershell
Get-AdfsGlobalAuthenticationPolicy

AdditionalAuthenticationProvider  : {}
DeviceAuthenticationEnabled   : False
PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}
PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}
WindowsIntegratedFallbackEnabled  : True

Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"
 ```

![自訂 displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)

我們現在看到顯示訊息已變更。

![自訂 displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)

## <a name="additional-references"></a>其他參考資料

[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
