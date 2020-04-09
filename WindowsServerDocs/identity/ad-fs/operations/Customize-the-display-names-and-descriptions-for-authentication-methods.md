---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: 自訂驗證方法的顯示名稱和描述
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 218643bbd5ada63b6bee2b91a7bace3f9959c0bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816441"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>自訂驗證方法的顯示名稱和描述 


若要自訂驗證方法的顯示名稱和描述，您可以使用 `Set-AdfsAuthenticationProviderWebContent` PowerShell Cmdlt。  若要使用此 Cmdlt，您必須先取得您想要自訂的驗證方法的名稱。  作法是使用 `Get-AdfsGlobalAuthenticationPolicy`。  在下面的範例中，我們看到，在頁面的 [登入]\-中顯示下列內容：「使用 x.509 憑證登入」。  我們想要為使用者簡化這行文字。  
  
![自訂 displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
首先，我們取得驗證方法的名稱，然後編輯顯示的文字。  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![自訂 displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
我們現在看到顯示訊息已變更。  
  
![自訂 displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
