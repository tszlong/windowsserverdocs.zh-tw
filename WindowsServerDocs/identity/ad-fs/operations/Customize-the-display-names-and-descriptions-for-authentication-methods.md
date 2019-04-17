---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: "自訂的顯示名稱及的驗證方法描述"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>自訂的顯示名稱及的驗證方法描述 

>適用於：Windows Server 2016、Windows Server 2012 R2

若要自訂的顯示名稱及描述您可以使用的驗證方法`Set-AdfsAuthenticationProviderWebContent`PowerShell cmdlt。  以使用此 cmdlt，您必須先取得您想要自訂的驗證方法的名稱。  這可以使用`Get-AdfsGlobalAuthenticationPolicy`。  我們看到以下範例，我們 sign\ 中在頁面上，以下顯示:「登入使用 x.509」。  我們希望此簡化的使用者。  
  
![自訂顯示名稱](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
讓第一次我們收到的驗證方法的名稱，然後我們編輯所顯示的文字。  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![自訂顯示名稱](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
現在我們看到已經我們顯示的訊息。  
  
![自訂顯示名稱](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
