---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: 自訂的顯示名稱和描述的驗證方法
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b9702873d42e0a72e510ac022d8d7fb04b45dab9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189175"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>自訂的顯示名稱和描述的驗證方法 


若要自訂驗證方法的顯示名稱和描述，您可以使用 `Set-AdfsAuthenticationProviderWebContent` PowerShell Cmdlt。  若要使用此 Cmdlt，您必須先取得您想要自訂的驗證方法的名稱。  作法是使用 `Get-AdfsGlobalAuthenticationPolicy`。  在下列範例中我們發現，在我們登\-在頁面中，顯示下列文字：「使用 X.509 憑證登入」。  我們想要為使用者簡化這行文字。  
  
![自訂顯示名稱](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
首先，我們取得驗證方法的名稱，然後編輯顯示的文字。  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![自訂顯示名稱](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
我們現在看到顯示訊息已變更。  
  
![自訂顯示名稱](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
