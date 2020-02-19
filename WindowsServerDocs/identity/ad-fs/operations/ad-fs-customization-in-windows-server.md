---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Windows Server 2016 中的 AD FS 自訂
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f7402cf549a7a2fb4b112bf92b36182f882b9d73
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465242"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Windows Server 2016 中的 AD FS 自訂


為了回應使用 AD FS 的組織所提供的意見反應，我們已新增額外的工具，以自訂受 AD FS 保護之個別應用程式的使用者登入體驗。  
除了指定每個應用程式的 web 內容（例如描述文字和連結）之外，您現在還可以為每個應用程式指定整個 web 主題。  這包括標誌、圖例、樣式表單或整個 onload 檔案。  
  
## <a name="global-settings"></a>全域設定    
針對一般的全域設定，您可以參考自訂 Windows Server 2012 R2 中 AD FS 隨附的[AD FS 登入頁面](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="pre-requisites"></a>必要條件  
在嘗試本檔中所述的程式之前，必須先具備下列必要條件。  
  
-   Windows Server 2016 TP4 或更新版本中的 AD FS  
  
## <a name="configure-ad-fs-relying-parties"></a>設定 AD FS 信賴憑證者  
您可以使用下列 PowerShell 範例來設定每個信賴憑證者登入 web 元素和主題：  
  
### <a name="customize-messages"></a>自訂訊息  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>自訂公司名稱、標誌和影像  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>自訂整頁  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>自訂主題和先進的自訂主題  
  
針對自訂主題，請參閱[自訂 AD FS 登入頁面](https://technet.microsoft.com/library/dn280950.aspx)和自[定義 AD FS 登入頁面。](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>為每個 RP 指派自訂 web 主題  
  
若要為每個 RP 指派自訂主題，請使用下列程式：  
  
1. 在 AD FS 中，建立新的主題做為預設全域主題的複本  
`New-AdfsWebTheme -Name AppSpecificTheme -SourceName default`  
2. 匯出自訂的主題  
`Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme`  
3. 自訂主題檔案（影像、css、onload）-在您慣用的編輯器中，或取代檔案  
4. 從檔案系統將自訂檔案匯入 AD FS （以新主題為目標）  
`Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}`  
5. 將新的自訂主題套用到特定 RP （或 RP）  
`Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme`  
  
## <a name="home-realm-discovery"></a>主領域探索  
針對主領域探索自訂，請參閱[自訂 AD FS 登入頁面](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="updated-password-page"></a>已更新密碼頁面  
如需自訂 [更新密碼] 頁面的詳細資訊，請參閱[自訂 AD FS 登入頁面](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="customizing-and-alternate-ids"></a>自訂和替代識別碼  
使用者可以使用 Active Directory Domain Services （AD DS）所接受的任何形式的使用者識別碼，登入 Active Directory 同盟服務（AD FS）啟用的應用程式。 這些包含使用者主體名稱（Upn）（johndoe@contoso.com）或網域限定的 sam 帳戶名稱（contoso\johndoe 或 com\johndoe）。  如需這種情況的詳細資訊，請參閱設定[替代登入識別碼。](Configuring-Alternate-Login-ID.md)  
  
您可能需要自訂 AD FS 登入頁面，以提供使用者有關替代登入識別碼的一些提示。 若要這麼做，您可以新增自訂的登入頁面描述以取得詳細資訊，請參閱[自訂 AD FS 登入頁面。](https://technet.microsoft.com/library/dn280950.aspx)   
  
您也可以在 [使用者名稱] 欄位中自訂 [以組織帳戶登入] 字串來執行這項操作。  如需相關資訊，請參閱[AD FS 登入頁面的 Advanced 自訂](https://technet.microsoft.com/library/dn636121.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
