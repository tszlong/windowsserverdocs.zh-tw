---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: "AD FS 登入頁面自訂的錯誤訊息"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a015c27f784d5b1f488f984450612820d4a06b1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>AD FS 登入頁面自訂的錯誤訊息  

>適用於：Windows Server 2016、Windows Server 2012 R2

您可以設定可以量身打造的自訂的錯誤訊息到您的組織。 下圖顯示自訂的錯誤頁面描述與一般錯誤訊息。 使用下列的 Windows PowerShell cmdlet 來自訂您的錯誤訊息。  
  
![自訂錯誤](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>自訂錯誤碼頁面  
若要自訂錯誤碼頁面上，使用下列 Windows PowerShell cmdlet 和語法。  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>自訂的一般錯誤訊息  
若要自訂的一般錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>自訂授權錯誤訊息  
若要自訂授權錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>自訂裝置驗證錯誤訊息  
若要自訂裝置驗證錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>來自訂郵件錯誤訊息的支援  
您可以設定 AD FS 支援電子郵件地址。 如果設定，AD FS 自動顯示以電子郵件傳送的錯誤的詳細資料終端使用者的連結。  
  
若要自訂支援的電子郵件的錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>自訂信賴的派對授權訊息  
您可以設定 AD FS 信賴的派對授權錯誤訊息。  
  
若要自訂信賴廠商錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)    
