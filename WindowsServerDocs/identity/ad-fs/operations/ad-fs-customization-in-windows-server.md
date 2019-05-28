---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Windows Server 2016 中的 AD FS 自訂
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b5aa22ad99529d99e2d7381a434916e8e749f185
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188759"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Windows Server 2016 中的 AD FS 自訂


回應意見反應與組織使用 AD FS，在中，我們已新增額外的工具，來自訂使用者的登入 AD FS 保護的個別應用程式的體驗。  
現在除了指定每個應用程式 web 內容，例如描述文字和連結，您可以指定每個應用程式的整個網頁佈景主題。  這包括標誌、 圖例、 樣式表或整個 onload.js 檔案。  
  
## <a name="global-settings"></a>全域設定    
一般的全域設定中，您可以參考以[Customizing the AD FS sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) ，隨附於 Windows Server 2012 R2 中 AD FS。  
  
## <a name="pre-requisites"></a>必要條件  
必須要具備下列必要條件，才能嘗試這份文件中所述的程序。  
  
-   在 Windows Server 2016 TP4 或更新版本的 AD FS  
  
## <a name="configure-ad-fs-relying-parties"></a>設定 AD FS 信賴憑證者的合作對象  
每一個 web 登入信賴憑證者合作對象項目和佈景主題可以設定使用下列 PowerShell 範例：  
  
### <a name="customize-messages"></a>自訂訊息  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>自訂公司名稱、 標誌和映像  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>自訂整個頁面  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>自訂佈景主題與進階自訂佈景主題  
  
如自訂佈景主題，請參閱[Customizing the AD FS sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)和[Advanced Customization of AD FS 登入頁面。](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>指派每個 RP 的自訂網頁佈景主題  
  
若要指派每個 RP 自訂佈景主題，請使用下列程序：  
  
1. 以預設值，AD FS 中的全域佈景主題的複本建立新的佈景主題  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code> 2.  匯出自訂的佈景主題 <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>  
3. 自訂佈景主題檔案 （影像、 css、 onload.js）-在您最愛的編輯器中，或取代檔案 4。 從檔案系統匯入自訂的檔案，對 AD FS （目標設為新的佈景主題） <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>  
5. 將新的自訂佈景主題套用至特定 RP （或 RP） <code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>主領域探索  
自訂如主領域探索[Customizing the AD FS sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="updated-password-page"></a>更新的密碼頁面  
如需自訂更新密碼頁面的詳細資訊，請參閱[Customizing the AD FS sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="customizing-and-alternate-ids"></a>自訂和替代識別碼  
使用者可以登入 Active Directory Federation Services (AD FS)-功能的應用程式使用任何形式的可接受的 Active Directory 網域服務 (AD DS) 的使用者識別碼。 其中包括使用者主體名稱 (Upn) (johndoe@contoso.com) 或網域限定 sam 帳戶名稱 （contoso\johndoe 或 contoso.com\johndoe）。  如需詳細資訊，請參閱上[設定替代登入識別碼。](Configuring-Alternate-Login-ID.md)  
  
此外您可以自訂 AD FS 登入頁面，讓使用者的一些秘訣替代登入識別碼。 您可以藉由新增自訂的登入頁面描述，如需詳細資訊，請參閱執行[自訂 AD FS 登入頁面。](https://technet.microsoft.com/library/dn280950.aspx)   
  
您也可以執行此自訂使用者名稱欄位上方的 「 使用組織帳戶登入 」 字串。  如需此，請參閱[Advanced Customization of AD FS sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
