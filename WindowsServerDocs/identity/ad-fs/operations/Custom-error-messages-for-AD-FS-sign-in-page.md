---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: AD FS 登入頁面的自訂錯誤訊息
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a015c27f784d5b1f488f984450612820d4a06b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828979"
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>AD FS 登入頁面的自訂錯誤訊息  

>適用於：Windows Server 2016, Windows Server 2012 R2

您可以設定自訂錯誤訊息，以符合組織需求。 下圖顯示自訂的錯誤頁面描述，以及一般錯誤訊息。 使用下列 Windows PowerShell cmdlet 自訂錯誤訊息。  
  
![自訂錯誤](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>自訂錯誤頁面描述  
若要自訂錯誤頁面描述，請使用下列 Windows PowerShell cmdlet 和語法。  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>自訂一般錯誤訊息  
若要自訂一般錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>自訂授權錯誤訊息  
若要自訂授權錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>自訂裝置驗證錯誤訊息  
若要自訂裝置驗證錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>自訂支援電子郵件錯誤訊息  
您可以在 AD FS 中設定的支援電子郵件地址。 如果設定，AD FS 會自動顯示連結，以讓使用者以電子郵件傳送錯誤詳細資料。  
  
若要自訂支援電子郵件錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>自訂信賴憑證者授權訊息  
您可以在 AD FS 中設定信賴憑證者的合作對象授權錯誤訊息。  
  
若要自訂信賴憑證者的合作對象錯誤訊息，請使用下列 Windows PowerShell cmdlet 和語法。  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)    
